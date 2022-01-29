---
title: "Bootstrapping your own domain"
date: 2021-07-09T20:25:48+08:00
---
Bootstrapping in this context means registering for a domain name using an email address and then changing that email address to the one that belongs to the domain itself afterwards.

That's a mouthful. Here's an example. You use `personal@mail.com` to register for the `example.com` domain name. Once the domain is up, you create an `admin@example.com` email address. Finally, you switch the email for the domain registrar and DNS hosting account to `admin@example.com`, replacing the personal email address that you were using before.

The reason for doing this is to ensure you don't tie critical infrastructure to any personnel. It is helpful, especially for an enterprise or business organisation, as people join and leave the organisation over time. Therefore, you want to tie these critical infrastructures to a non-changing identity belonging to the domain instead.

There are two components in bootstrapping a domain and its infrastructure:

  1. The domain name itself.
  1. An email system.

Some vendors provide an end-to-end solution to set up everything on a single platform. But for those who prefer to have more flexibility in managing the domain and email system separately, this post could help you.

Before getting started, make sure you have an email address from a third-party provider, e.g. Microsoft Outlook, Gmail, Yandex, Yahoo, etc. I'll be referring to these email addresses as _managed_ email addresses from here onwards. Almost all providers that offer DNS hosting services or email services require some ways to contact you, and having a managed email address on hand help will help a lot.

There are three steps:

  1. Using a managed email address, register for the domain via a DNS hosting service provider (NameSilo, Cloudflare, Google Domains, Amazon Route53, etc.).

  1. Using a managed email address, register for an email service (Protonmail, Migadu, Purelymail, etc.). After you have an email service, add the relevant SPF, DKIM, and DMARC DNS records to your registered domain.

  1. Now, you can create a mailbox for your domain's administrator user on your email service. Go ahead and create one. Then, change the managed email address on the DNS hosting and email service provider to this new email address. And that completes the entire bootstrapping process.

Also to note, this creates circular dependencies between the DNS hosting and email service provider, therefore giving rise to potential lockout scenarios. But on the plus side, the domain is not tied to any managed email address from a third-party provider. Make sure there is an account recovery mechanism in place on both systems to avoid being locked out.

Some DNS hosting service providers do not allow you to change your email address or use the same personal account to manage other domains. In this case, you could create a different account on the provider using the new administrator email address and transfer the domain to this new account instead.
