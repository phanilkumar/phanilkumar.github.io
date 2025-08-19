---
title: "The Hidden Danger of Private Method Calls in Ruby: Why 'send' Can Break Your Code"
seoTitle: "Beware: Ruby 'send' Can Break Code"
seoDescription: "Discover the risks of using Ruby's `send` method for private calls and learn effective alternatives for safer, maintainable code"
datePublished: Tue Jul 08 2025 12:47:55 GMT+0000 (Coordinated Universal Time)
cuid: cmcuizjtu001o02jr5hh97i45
slug: the-hidden-danger-of-private-method-calls-in-ruby-why-send-can-break-your-code
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/DFtjXYd5Pto/upload/5f1c2ffae150ceb47296106d6e2cc708.jpeg
tags: ruby, ruby-on-rails

---

### **The Problem**

When working with Ruby modules and inheritance, developers often use the `send`method to call private methods across different classes. While this might seem like a clever workaround, it can lead to subtle bugs and maintenance issues.

### **The Issue Explained**

Consider this common pattern in Ruby applications:

```ruby
# Base module with private methods
module PaymentProcessor
  private

  def process_payment(amount)
    # Payment logic
    "Processed #{amount}"
  end

  def validate_transaction(user)
    # Validation logic
    user.valid?
  end
end

# Class that includes the module
class PaymentGateway
  include PaymentProcessor

  def handle_payment(amount, user)
    # Using send to call private methods
    result = send(:process_payment, amount)
    valid = send(:validate_transaction, user)

    return result if valid
  end
end
```

### **Why This Breaks**

The `send` method bypasses Ruby's method visibility rules, which can cause several problems:

1. **Method Signature Changes**: If the private method's signature changes, the `send`call won't be caught by the compiler
    
2. **Refactoring Nightmares**: IDEs and static analysis tools can't properly track these calls
    
3. **Debugging Difficulties**: Stack traces become confusing when methods are called via `send`
    
4. **Testing Complexity**: It's harder to mock or stub methods called via `send`
    

### The Solution

Instead of using `send`, make the methods public or use proper delegation:

```ruby
# Better approach: Make methods public
module PaymentProcessor
  def process_payment(amount)
    # Payment logic
    "Processed #{amount}"
  end

  def validate_transaction(user)
    # Validation logic
    user.valid?
  end
end

# Clean, readable code
class PaymentGateway
  include PaymentProcessor

  def handle_payment(amount, user)
    # Direct method calls - much cleaner!
    result = process_payment(amount)
    valid = validate_transaction(user)

    return result if valid
  end
end
```

### **Alternative: Proper Delegation**

If you need to maintain encapsulation, use proper delegation patterns:

```ruby
class PaymentGateway
  include PaymentProcessor

  def handle_payment(amount, user)
    # Delegate to a dedicated processor
    PaymentHandler.new.process(amount, user)
  end
end

class PaymentHandler
  def process(amount, user)
    result = process_payment(amount)
    valid = validate_transaction(user)

    return result if valid
  end

  private

  def process_payment(amount)
    "Processed #{amount}"
  end

  def validate_transaction(user)
    user.valid?
  end
end
```

### **Key Takeaways**

1. **Avoid** `send` for method calls: It breaks encapsulation and makes code harder to maintain
    
2. **Make methods public when appropriate**: Don't hide methods that need to be called from other classes
    
3. **Use proper delegation**: Create dedicated classes or modules for shared functionality
    
4. **Follow Ruby conventions**: Let the language's visibility rules work for us, not against us
    

### **Real-World Impact**

In production applications, `send` calls can lead to:

* Runtime errors that are hard to debug
    
* Performance issues due to method lookup overhead
    
* Code that's difficult to refactor or test
    
* Maintenance headaches for future developers
    

The fix is simple: replace `send` calls with direct method calls and adjust method visibility accordingly.