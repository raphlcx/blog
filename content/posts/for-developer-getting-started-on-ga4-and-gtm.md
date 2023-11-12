---
title: "For developer: Getting started on GA4 and GTM"
date: 2023-11-11T12:25:12+08:00
---

How to setup Google Analytics 4 (GA4) and Google Tag Manager (GTM) on your local development and production website.

We'll use `www.example.com` as our example site.

## Steps (GA4)

GA4 (or GA) is a platform that tracks and reports analytical data on web/app traffic.

  1. Create your Google Analytics (GA) account. Name the account as "Personal" if it's under your personal project, otherwise use your business name.

  1. Create a GA property under the same account. Name the property as the site domain, i.e. `www.example.com`.

  1. Create a web data stream for the property. Name it as "example-www". The measurement ID i.e. "G-XXX" is the GA4 tag ID. Note this down because we'll use it later.

  1. You are done setting up GA4!

## Steps (GTM)

GTM is a tool for deploying GA4 to your website.

  1. Create your GTM account. Name the account as "Personal" if it's under your personal project, otherwise use your business name.

  1. Create a GTM container under the same account. Name it as the site domain, i.e. `www.example.com`.

  1. Navigate into the container, and choose to add new tag.

  1. You'll be adding the GA4 tag that you want to deploy with GTM.

      1. Name the new tag as "GA4 - G-XXX", where "G-XXX" is the GA4 tag ID.
      1. Under tag configuration, add a "Google Tag". Then, fill in the tag ID field with the GA4 tag ID.
      1. Under triggering, choose "Initialization - All Pages".
      1. Save the changes.

  1. Submit GTM changes by publishing and creating a new version.

  1. You are done setting up GTM!

## Live: GA4 and GTM

  1. Visit the GTM admin page, look for the instructions to install GTM. The instructions should display the required code snippet that you'll have to add to the `<head>` and `<body>` sections on your website.

  1. Add required code snippets to your website.

  1. You have just installed GTM on your site, and you should see analytics data flowing into your Google Analytics dashboard whenever your site receives traffic.

Note: You might also want to add a conditional check on your website so that it includes GTM only when it is in production environment. It prevents your local development machine from sending analytical data to GA4 every time you open up your developmental website. Alternatively, you could also include GTM in all environments, and define internal traffic rules and data filters on GA4, but as a developer, a conditional check would be much more straightforward.
