---
title: "ActiveJob Object Mutability: The Photocopy Problem"
seoTitle: "ActiveJob Object Mutability Overview"
seoDescription: "Learn about the photocopy problem in ActiveJob serialization, its challenges, and solutions for handling object mutability effectively"
datePublished: Thu Aug 21 2025 07:27:05 GMT+0000 (Coordinated Universal Time)
cuid: cmel2wg37001b02ihd3dd2pam
slug: activejob-object-mutability-the-photocopy-problem
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/Bwv6n__ZnWE/upload/e5aa6ed425a6b428296fe20f2dddcf92.jpeg
tags: rails

---

One of the most confusing aspects of ActiveJob custom serializers isn't how to create them—it's understanding that **deserialized objects are completely new instances**.

Think of it like photocopying a document. The copy has the same content, but it's not the same piece of paper. Writing on the copy doesn't change the original.

## The Problem in Action

```ruby
# Create a counter object
counter = Counter.new(5)
puts counter.object_id  # => 12345

class MyJob < ApplicationJob
  def perform(counter)
    puts counter.object_id    # => 67890 (DIFFERENT!)
    puts counter.value        # => 5 (same content)
    
    counter.value = 10        # Change the copy
  end
end

MyJob.perform_later(counter)
puts counter.value  # => Still 5, not 10! 😱
```

## Real-World Gotchas

### 1\. Expecting Mutations to Persist

**❌ This Won't Work:**

```ruby
shopping_cart = ShoppingCart.new
shopping_cart.total = 100

class UpdateCartJob < ApplicationJob
  def perform(cart)
    cart.total = 200  # Changes a COPY, not the original
  end
end

UpdateCartJob.perform_later(shopping_cart)
puts shopping_cart.total  # Still 100!
```

**✅ Fix: Use Database Storage**

```ruby
class UpdateCartJob < ApplicationJob
  def perform(cart_id)
    cart = ShoppingCart.find(cart_id)  # Fresh from database
    cart.update!(total: 200)           # Persists changes
  end
end

UpdateCartJob.perform_later(shopping_cart.id)
```

### 2\. Using Objects as Unique Keys

**❌ This Won't Work:**

```ruby
processed_items = Set.new
money = Money.new(100, 'USD')
processed_items << money

class ProcessJob < ApplicationJob
  def perform(money, processed_items)
    if processed_items.include?(money)  # Always FALSE!
      puts "Already processed"
    else
      puts "Processing..."  # Always runs
    end
  end
end
```

**✅ Fix: Use Value-Based Keys**

```ruby
processed_amounts = Set.new
money = Money.new(100, 'USD')
processed_amounts << "#{money.amount}-#{money.currency}"

class ProcessJob < ApplicationJob
  def perform(money, processed_amounts)
    money_key = "#{money.amount}-#{money.currency}"
    if processed_amounts.include?(money_key)  # Now works!
      puts "Already processed"
    end
  end
end
```

### 3\. Shared Counters and State

**❌ This Won't Work:**

```ruby
request_counter = RequestCounter.new
request_counter.count = 5

class TrackRequestJob < ApplicationJob
  def perform(counter)
    counter.increment!  # Increments the COPY
  end
end

# Each job gets its own copy
TrackRequestJob.perform_later(request_counter)
TrackRequestJob.perform_later(request_counter)
puts request_counter.count  # Still 5!
```

**✅ Fix: Use External Storage**

```ruby
class TrackRequestJob < ApplicationJob
  def perform(user_id)
    Redis.current.incr("requests:#{user_id}")  # Shared counter
  end
end
```

## The Golden Rules

1. **Treat serialized objects as read-only copies**
    
2. **Store mutable state externally** (database, Redis, files)
    
3. **Compare values, not object identity**
    
4. **Pass IDs instead of objects when you need to mutate**
    

## Quick Test

Ask yourself: "If I photocopied this document and gave you the copy, would you expect changes to your copy to appear on my original?"

If the answer is no, don't expect it to work with ActiveJob either.

## Design for Immutability

The best objects for ActiveJob serialization are immutable value objects:

```ruby
class Money
  attr_reader :amount, :currency
  
  def initialize(amount, currency)
    @amount = amount
    @currency = currency
    freeze  # Make immutable
  end
  
  def ==(other)
    amount == other.amount && currency == other.currency
  end
end
```

## Conclusion

ActiveJob serialization creates **new objects with the same values**, not references to the same objects. Understanding this "photocopy principle" will save you hours of debugging mysterious behavior where changes seem to disappear.

Design your serialized objects to be immutable value containers, and use external storage for any state that needs to persist or be shared between jobs.