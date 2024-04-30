---
title: "Organisation mailing groups"
date: 2024-04-28T00:07:53+08:00
---

Case: Abel has recently launched his new company and anticipates hiring finance, tech, human resources, social marketing, and other teams in the coming months. To prepare for the company's expansion, he subscribes to cloud vendor software and services. Abel owns the domain `example.com` and has set up his email address as `abel@example.com`, which he uses to sign up for all vendor services.

Abel's proactive approach is commendable.

However, looking ahead, there's a potential issue:

What if, in a year, Abel decides to leave the company, leaving behind the teams he has hired? All the vendor access is tied to Abel's email address (`abel@example.com`). This could pose a challenge for the IT team when migrating and recovering access. Will the IT team transfer access to the next executive, Cain, who uses `cain@example.com`?

Perhaps a better alternative is to avoid tying vendor access to a specific individual in the first place. This would ensure the smooth operation of systems even with employee turnover.

## Step 1: Create a system user

Firstly, create a system user to manage the company's systems. Although you are the legal owner of the company, you won't necessarily need the extensive privileges that come with the owner role for your daily operations.

Let's call this system user `admin@example.com`. This user will be the primary account for provisioning other users.

## Step 2: Implement mailing groups

With the system user in place, should we use it to sign up for vendor access?

Not yet. Your future finance or tech team will likely have more than one member who needs to receive emails. If you use only the system user, only the person with access to the system user will be able to receive messages, and we don't want to share the system user's credentials with everyone in the company.

The solution is to create different mailing groups for different functional departments.

For example:

- `finance@example.com` for finance and accounting.
- `tech@example.com` for information technology.
- `media@example.com` for social media platforms.
- `people@example.com` for human resources.
- `careers@example.com` for recruitment.
- `support@example.com` for customer support.
- `hello@example.com` for general inquiries.

Use these mailing groups to sign up as the owner of related vendor services. For each mailing group, add the relevant members. This setup offers several benefits:

- All members in the group can receive email messages.
- When a member leaves the company, there's no need to migrate owner access to vendor services, as owner access is managed by the mailing group.

On some occasions, you may encounter a situation where you need to sign up for multiple accounts with the same vendor, but the vendor requires unique email addresses for each account.

In such cases, you can use plus addressing to sign up for these accounts while still using a single mailing group. For example, you can sign up for two AWS accounts using `tech+aws.one@example.com` and `tech+aws.two@example.com`.

## In summary

Use a system user and mailing groups to manage access to systems. Avoid the use of individual human users for this purpose.
