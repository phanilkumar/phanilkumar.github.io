---
title: "Ingress Configuration"
datePublished: Fri Nov 08 2024 15:23:26 GMT+0000 (Coordinated Universal Time)
cuid: cm38vzeom000909l4anaz24gr
slug: ingress-configuration
canonical: https://guides.rubyonrails.org/action_mailbox_basics.html#ingress-configuration
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/SYTO3xs06fU/upload/b7f6cc717a8dbea3b8f2abf16b726189.jpeg
tags: ruby, rails, ruby-on-rails, ruby-on-rails-developer

---

**Configuring ingress involves**  
1\. Setting up credentials  
2\. Endpoint information for the chosen email service.

  
Some supported ingresses are:

* Exim
    
* Mailgun
    
* Mandrill
    
* Postfix
    
* Postmark
    
* Qmail
    

Mandrill Ingress setup:

Use `rails credentials:edit` to add your API key to your application's encrypted credentials under `action_mailbox.mandrill_api_key`, where Action Mailbox will automatically find it:

```ruby
action_mailbox:
  mandrill_api_key: ...
```

Alternatively, provide API key in the `MANDRILL_INGRESS_API_KEY` environment variable.

Tell Action Mailbox to accept emails from Mandrill:

```ruby
# config/environments/production.rb
config.action_mailbox.ingress = :mandrill
```