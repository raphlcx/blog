---
title: "Migrating website domain"
date: 2021-11-07T17:48:32+08:00
---
I have recently changed the domain for this site, from `blog.ctfis.art` to `blog.raphlcx.com`, as part of an effort to consolidate my online presence and identity. This post shares the steps in performing the migration.

For context, the `ctfis.art` domain was registered on NameSilo, while the DNS records and website hosting was fully managed using Netlify.

So, the steps:

  1. Register the new domain, `raphlcx.com`, via a registrar. I used Cloudflare.
  1. On Netlify, register the new site or domain alias for the deployed site. But this time, I want to retain DNS records management on Cloudflare instead, so I opt to not use Netlify DNS here.
  1. On Cloudflare, create a CNAME record for `blog.raphlcx.com` pointing to the Netlify deployed site.
  1. On NameSilo, revert to use its default name servers, and configure a 301 permanent redirect from `blog.ctfis.art` to `blog.raphlcx.com`.
  1. Wait for maximum 48 hours for the DNS changes on NameSilo to be propagated. During this period, the site will be reachable from both domains. `blog.raphlcx.com` will be via Cloudflare DNS, while `blog.ctfis.art` via NameSilo, then Netlify DNS.
  1. Once the redirect is working, remove `ctfis.art` domain configuration on the Netlify deployed site and Netlify DNS.
  1. Disable auto renewal for `ctfis.art` domain on NameSilo.
  1. On Google Search Console, migrate the site domain to the new domain.

Netlify automatically provisions TLS certificate for the domain that it manages via Netlify DNS. For the previous domain, I presume the certificate for it will fail to renew once it expires, and be removed from Netlify eventually.
