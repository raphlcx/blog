---
title: "Apex domains and DNS zones"
date: 2021-06-01T23:55:12+08:00
tags: [aws]
---
Quiz! Which of the following is an apex domain?

  - `example.com`
  - `www.example.com`

Did you pick `example.com`?

But, it is not necessarily just `example.com`. The domain `www.example.com` could be an apex domain too. It all depends on which DNS zone contains the DNS records.

We define DNS records in a DNS zone. A zone is simply a conceptual term to denote a folder for the records that it holds.

A zone has a start of authority (SOA) record. In the zone, we create DNS records for the apex domain. Here, we shall name our zone as "Zone 1":

```
; Zone 1

; 'SOA' record for this zone
example.com SOA ...

; 'A' record for apex domain
example.com A ...

; 'A' record for www subdomain
www.example.com A ...
```

In this zone, `example.com` is an apex domain because it has an SOA with the same name. `www.example.com` is not an apex domain, well, because it doesn't have an SOA to itself.

Now, what if `www.example.com` has its zone? In "Zone 1", we shall delegate domain name lookup for `www.example.com` to another DNS zone. We will call this zone "Zone 2":

```
; Zone 1 (updated)

example.com SOA ...

example.com A ...

; Delegate resolution for www subdomain to Zone 2
www.example.com NS ...
```

```
; Zone 2

www.example.com SOA ...

www.example.com A ...
```

In Zone 1, `example.com` is still an apex domain because of its SOA record.

But thanks to Zone 2, `www.example.com` is also an apex domain now, in this particular zone. Notice it now has an SOA. Although back in Zone 1, we will still not consider `www.example.com` as an apex domain, owing to the differing SOA.

So there you have it. A DNS zone determines whether a domain or a subdomain is an apex domain.

But why does apex or non-apex domain matter? Because of the CNAME record. An apex domain cannot have a CNAME record for it.

I had a similar setup in subdomain delegation on Amazon Route53. In Zone 2, I tried to CNAME `www.example.com`, which I wasn't aware was an apex domain, to an Application Load Balancer's DNS name. Naturally, Route53 refused to create the CNAME record, but fortunately, I could configure a Route53 alias record for it.

From another perspective, recall that in the zone `.com`, your `example.com` is not an apex domain but a subdomain of `.com`. However, when `example.com` is in your dedicated zone, it becomes an apex domain.
