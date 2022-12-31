---
title: "Receiving emails with Google Workspace, Migadu, Amazon SES, and Microsoft Outlook"
date: 2022-12-31T12:36:17+08:00
---
I have a domain (let's call it `mydomain.com`) that I would like to receive emails on it. For the past few years, I have tried different methods to receive emails on the domain, with each successive trial aiming to save as much monetary cost as possible.

## Google Workspace

I initially started with Google Workspace, which is arguably one of the easier ways to get started with receiving emails on a custom domain.

A single user on the Business Starter plan costs $5 USD per month, so that's $60 USD per year.

I wanted to have 2 users on my domain, one for administration and another one for my own identity. But, that would cause the cost to grow linearly with the number of users created in the workspace.

That cost is not justified given all the users in the domain are only receiving at maximum 3 to 5 emails per month.

## Migadu

So, I looked into other email hosting services and came across a recommendation to try out Migadu. Migadu used to have a free plan, but owing to its growing operational cost, they discontinued it and introduced a micro plan costing $19 USD per year.

The one very good benefit from Migadu is that they don't charge based on the number of users or mailboxes.

It's user interface and feature set are also no-frills. I particularly like their branding and operating philosophy.

## Amazon SES

After a year and a half of using Migadu, I wanted to optimise the monetary cost further.

I've known about Amazon Simple Email Service (SES), but only came to know recently that it also has capability to act as a email receiver. It's pricing is usage-based, which suits my use case perfectly given the very-low email traffic that my domain has.

I set up MX record to forward my domain's emails to SES and created a receipt rule to forward the mails to my personal mailbox.

It worked flawlessly, but the received email's content in my personal mailbox is a plain JSON object, containing all the bunch of metadata and headers related to the email. It was not a format for a human user to consume.

## Microsoft Outlook

If I could set up MX to redirect emails to SES, technically I can also set it up to route the mails to my personal mailbox on Microsoft Outlook.

So, I created account aliases for my domain's users on my personal Outlook account, and then set up MX to redirect their emails to Outlook's mail server.

After that, I sent a test email from my personal email address to one of my domain user's email address, and it worked perfectly.

I have effectively reduced the cost of receiving emails on my domain to $0 per year, albeit the downside is that I'm allowing Microsoft to read all of these emails. But, maybe all these email hosting providers are reading them too.
