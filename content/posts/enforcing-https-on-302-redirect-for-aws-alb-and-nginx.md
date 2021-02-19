---
title: "Enforcing HTTPS for 302 redirect on AWS ALB and Nginx"
date: 2021-02-19T21:22:21+08:00
tags: [aws]
---
Another day, another venture on an AWS product. This time it revolves around Application Load Balancer (ALB).

Setting up some context.

I was working with my team mate to deploy a third party application (henceforth to be referred as the "app") in an EC2 instance. Traffic to this application is proxied by Nginx, also running in the same instance. Initially we created a self-signed TLS certificate and served this via Nginx. Any non-HTTPS traffics received by Nginx will be redirected to HTTPS, which then gets proxy-passed to the app via HTTP.

Soon after, we wanted to place an ALB in front of this EC2 instance. So, we provisioned a TLS certificate using AWS Certificate Manager (ACM), and attached this piece of certificate to the ALB. HTTPS now terminates on the ALB. Nginx no longer needs to serve that self-signed certificate anymore. It simply forwards the HTTP traffics that it receives to the app.

The setup of ALB with EC2 worked perfectly, but with only one glaring issue. Whenever we visit the app via its domain, e.g. `https://app.com`, the app will redirect us, via HTTP 302, to its login page for authentication. But, the redirect location was using HTTP scheme, i.e. `http://app.com/login`. This did not work, since our provisioned ALB is only listening on port 443. No port 80 was open.

We were perplexed. Was it a configuration issue on ALB? Or Nginx? Or simply the app has an errounous bug that hardcodes the redirection scheme to HTTP?

After scourging the net for similar redirection issue specific to the app, I came across a GitHub issue for it. Participants in the issue were suggesting that, the scheme used in a HTTP redirection depends solely on the proxy being used. The app is simply using the scheme of the request that it receives.

Traffic in ALB is routed to a target group, which contains the target where it would receive the traffic. In our case, the target is the EC2 instance. Looking at the flow of traffic in an ALB + EC2 setup:

```
Client       (https)> ALB
ALB          (http) > Target group
Target group (http) > Target
Target       (http) > Nginx
Nginx        (http) > The app
```

Since Nginx is receiving and forwarding HTTP traffic to the app, naturally the app returns a redirect URL in the same scheme.

The solution that we adopted eventually in enforcing HTTPS on the redirect URL, was to manually rewrite the scheme from HTTP to HTTPS. In the Nginx server configuration:

```
server {
  ...

  location / {
    ...
    proxy_pass http://app;
    proxy_redirect http:// https://;  # the rewrite
  }
}
```

This ensures all HTTP 302 redirect coming from the app is coverted to using HTTPS scheme.

I'm not entirely convinced that this is the *right way* to do it, but perhaps it is. It feels weird that we are forcing a rewrite of HTTP into HTTPS, while the traffic is still traversing back up target, target group, and finally to ELB in HTTP.
