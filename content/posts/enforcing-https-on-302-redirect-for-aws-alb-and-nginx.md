---
title: "Enforcing HTTPS for 302 redirect on AWS ALB and Nginx"
date: 2021-02-19T21:22:21+08:00
tags: [aws]
---
Another day, another venture on an AWS product. This time it revolves around Application Load Balancer (ALB).

I worked with my colleague to deploy a third-party application (the "app") to an Amazon EC2 instance. Traffic to this application is proxied by Nginx, also running in the same instance. We created a self-signed TLS certificate and served it via Nginx. We then set up an Nginx forwarding rule to proxy-pass all traffic to the app.

Soon after, we wanted to place an ALB in front of this EC2 instance. So, we provisioned a TLS certificate using AWS Certificate Manager (ACM) and attached the certificate to the ALB. The ALB then handled TLS termination. Nginx no longer needed to serve that self-signed certificate anymore and was only responsible to route the HTTP traffics that it receives to the app.

The setup of ALB with EC2 worked perfectly, but with only one glaring issue. Whenever we visit the app via its domain, e.g. `https://app.com`, the app will redirect us, via HTTP 302, to its login page for authentication. But, the redirect location was using an HTTP scheme, i.e. `http://app.com/login`. It caused an issue since our provisioned ALB is only listening on port 443. No port 80 was open.

We were perplexed. Was it a configuration issue on ALB? Or Nginx? Or simply the app has an erroneous bug that hardcodes the redirection scheme to HTTP?

After scouring the net for a similar redirection issue, I came across a GitHub issue for it. Commenters suggested that the scheme used in an HTTP redirection depends solely on the proxy in use. The app is simply using the same URI scheme from its requests.

ALB routes traffic to a target group. A target group contains the target where it would receive the traffic. In our case, the target is the EC2 instance. Looking at the flow of traffic in an ALB + EC2 setup:

```
Client       (https)> ALB
ALB          (http) > Target group
Target group (http) > Target
Target       (http) > Nginx
Nginx        (http) > The app
```

Since Nginx is forwarding HTTP traffic to the app, the app naturally returns a redirect URL in the same scheme.

To enforce HTTPS on the redirect, we rewrote the scheme from HTTP to HTTPS manually. In the Nginx server configuration:

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

Nginx will now convert all HTTP 302 redirects from the app to the HTTPS scheme.

I am still not convinced that this is the *right way* to do it, but perhaps it is. It feels weird that we are forcing a scheme rewrite from HTTP to HTTPS while the traffic is still traversing back up to target, target group, and the ALB in HTTP.
