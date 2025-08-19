---
title: "What is Action Mailbox?"
datePublished: Sun Nov 03 2024 06:01:06 GMT+0000 (Coordinated Universal Time)
cuid: cm316oyxb000109le9hul69do
slug: what-is-action-mailbox
canonical: https://guides.rubyonrails.org/action_mailbox_basics.html
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/aIYFR0vbADk/upload/067494b1f107ecd665671d08df5bf8ba.jpeg
tags: ruby, ruby-on-rails, ruby-on-rails-developer

---

## **Action Mailbox**

* Action Mailbox routes incoming emails to controller-like mailboxes for processing in your Rails application. Action Mailbox is for receiving email, while [Action Mailer](https://guides.rubyonrails.org/action_mailer_basics.html) is for *sending* them.
    
* The inbound emails are routed asynchronously using [Active Job](https://guides.rubyonrails.org/active_job_basics.html) to one or several dedicated mailboxes.
    
* These emails are turned into [`InboundEmail`](https://api.rubyonrails.org/v7.2.2/classes/ActionMailbox/InboundEmail.html) records using [Active Record](https://guides.rubyonrails.org/active_record_basics.html), which are capable of interacting directly with the rest of your domain model.
    
* [InboundEmail](https://api.rubyonrails.org/v7.2.2/classes/ActionMailbox/InboundEmail.html) records also provide lifecycle tracking, storage of the original email via [Active Storage](https://guides.rubyonrails.org/active_storage_overview.html), and responsible data handling with on-by-default [incineration](https://guides.rubyonrails.org/action_mailbox_basics.html#incineration-of-inboundemails).
    
* Action Mailbox ships with ingresses which enable your application to receive emails from external email providers such as Mailgun, Mandrill, Postmark, and SendGrid.
    
* You can also handle inbound emails directly via the built-in Exim, Postfix, and Qmail ingresses.