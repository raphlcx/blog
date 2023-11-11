---
title: "For developer: Getting started on GA4 and GTM"
date: 2023-11-11T12:25:12+08:00
draft: true
---

How to setup Google Analytics 4 (GA4) and Google Tag Manager (GTM) on your local development and production website.

We'll use `www.example.com` as our example site.

## Steps (GA4)

GA4 (or GA) is a platform that tracks and reports analytical data on web/app traffic.

  1. Create your Google Analytics (GA) account. Name the account as "Personal" if it's under your personal project, otherwise use your business name.

  1. Create a GA property under the same account. Name the property as the site domain, i.e. `www.example.com`.

  1. Create a web data stream for the property. Name it as "example-www". The measurement ID i.e. "G-XXX" is the GA4 tag ID. Note this down because we'll use it later.

  1. Define the internal traffic for this data stream. This step is for identifying analytics data sent from local development.

      1. Select the data stream.
      1. Choose to configure the tag settings.
      1. Choose to define internal traffic.
      1. Create a new internal traffic rule, name it as "Local development".
      1. On the internal traffic rule, add an IP address condition for CIDR matching `127.0.0.0/8`.

  1. Define data filter to exclude development and internal traffic on the property.

      1. Visit the data filters page.
      1. Create a data filter of type "Developer traffic", name it as "Developer traffic" as well. Set filter operation to "Exclude" and select "Active" under filter state.
      1. Create a data filter of type "Internal traffic", name it as "Internal traffic" as well. Set filter operation to "Exclude" and select "Active" under filter state.

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
      1. Publish the change.

  1. You are done setting up GTM!

## Live: GA4 and GTM

  1. Visit the GTM admin page, look for the instructions to install GTM. The instructions should display the required code snippet that you'll have to add to the `<head>` and `<body>` sections on your website.

  1. Add required code snippets to your website.

  1. You have just installed GTM on your site, and you should see analytics data flowing into your Google Analytics dashboard whenever your site receives traffic.
