---
title: "Action Mailbox Setup"
datePublished: Wed Nov 13 2024 11:31:03 GMT+0000 (Coordinated Universal Time)
cuid: cm3fsvt3z00090al25h0sh8x7
slug: action-mailbox-setup
canonical: https://guides.rubyonrails.org/action_mailbox_basics.html#setup
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/bJhT_8nbUA0/upload/0fe013395096df7622f5229ec293237e.jpeg
tags: ruby-on-rails, ruby-on-rails-developer

---

Action Mailbox has a few moving parts. First, run the installer. Next, choose and configure an ingress for handling incoming email. Then ready to add Action Mailbox routing, create mailboxes, and start processing incoming emails.

To start, let's install Action Mailbox:

```ruby
rails action_mailbox:install
```

This will create an `application_mailbox.rb` file and copy over migrations.

```ruby
rails db:migrate
```

This will run the Action Mailbox and Active Storage migrations.

The Action Mailbox table `action_mailbox_inbound_emails` stores incoming messages and their processing status.

At this point, start Rails server and check out `http://localhost:3000/rails/conductor/action_mailbox/inbound_emails`.

The next step is to configure an ingress in Rails application to specify how incoming emails should be received.