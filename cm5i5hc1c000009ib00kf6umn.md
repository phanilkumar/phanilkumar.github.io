---
title: "Action View Helpers: Simplify Your Ruby on Rails Development"
seoTitle: "Streamline Rails with Action View Helpers"
seoDescription: "Learn how Action View Helpers can simplify formatting dates, numbers, and text in Ruby on Rails development"
datePublished: Sat Jan 04 2025 12:18:39 GMT+0000 (Coordinated Universal Time)
cuid: cm5i5hc1c000009ib00kf6umn
slug: action-view-helpers-simplify-your-ruby-on-rails-development
canonical: https://guides.rubyonrails.org/action_view_helpers.html#forms
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/Oaqk7qqNh_c/upload/00448a666eb1c60e6e9b090ef2bcee28.jpeg
tags: ruby, ruby-on-rails

---

# ***Formatting:***

* *Dates*
    
* *Numbers*
    
* *Text*
    

```ruby

distance_of_time_in_words(Time.current, 15.seconds.from_now)
# => less than a minute
distance_of_time_in_words(Time.current, 15.seconds.from_now, include_seconds: true)
# => less than 20 seconds
time_ago_in_words(3.minutes.from_now) # => 3 minutes

number_to_currency(1234567890.50) # => $1,234,567,890.50 (Formats a number into a currency string)

number_to_human(1234)    # => 1.23 Thousand
number_to_human(1234567) # => 1.23 Million
number_to_human_size(1234)    # => 1.21 KB
number_to_human_size(1234567) # => 1.18 MB

number_to_percentage(100, precision: 0) # => 100%  (Formats a number as a percentage string.)
number_to_phone(1235551234) # => 123-555-1234 (US by default)
number_with_delimiter(12345678) # => 12,345,678 (Formats a number with grouped thousands using a delimiter.)

number_with_precision(111.2345)               # => 111.235 (precision defaults to 3)
number_with_precision(111.2345, precision: 2) # => 111.23

excerpt("This is a very beautiful morning", "very", separator: " ", radius: 1)
# => ...a very beautiful...

excerpt("This is also an example", "an", radius: 8, omission: "<chop> ")
#=> <chop> is also an example

pluralize(1, "person") # => 1 person
pluralize(2, "person") # => 2 people
pluralize(3, "person", plural: "users") # => 3 users

truncate("Once upon a time in a world far far away")
# => "Once upon a time in a world..."
truncate("Once upon a time in a world far far away", length: 17)
# => "Once upon a ti..."
truncate("one-two-three-four-five", length: 20, separator: "-")
# => "one-two-three..."
truncate("And they found that many people were sleeping better.", length: 25, omission: "... (continued)")
# => "And they f... (continued)"
truncate("<p>Once upon a time in a world far far away</p>", escape: false)
# => "<p>Once upon a time in a wo..."

word_wrap("Once upon a time", line_width: 8)
# => "Once\nupon a\ntime"
```