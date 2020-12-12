---
title: "Landing Page With Amazon S3, CloudFront, AWS Certificate Manager"
date: 2020-10-07T20:54:14+08:00
tags: [aws]
---
Last week, I created a simple landing page for a business, utilising solely AWS services, and I want to share that experience in this post.

The landing page is a single page view, detailing what the business does, with a CTA embedded on the same page. Simple enough, I bootstrapped the site using Hugo, prepared a theme, and positioned the content accordingly on the page. This took only a few minutes to get done with.

Deployment is the part that I want to experiment more on. I have been wanting to run a static site entirely using AWS services, and this is just the right time that I get to work on this.

Skimming the details, the action items are:

  1. Create an S3 bucket. Just a bucket, without website hosting feature enabled.

  1. Create an IAM user, with the necessary permission to deploy Hugo sites into the bucket.

  1. Create a certificate on AWS Certificate Manager (ACM) for the custom domain that you want to use. Of course, this means you need to register the domain somewhere else, or via Amazon Route 53, which is slightly pricier compared to standard market rates.

  1. Create a CloudFront distribution, specifying the S3 bucket as the origin, the custom domain that you want to use as the alias, and attaching the certificate from ACM.

  1. On your domain service provider, add a CNAME record for the custom domain, pointing to the CloudFront distribution's domain.

The S3 bucket does not need to be publicly exposed. With CloudFront Origin Access Identity (OAI), you only need to allow the OAI to read the contents in the bucket, since CloudFront is serving as the public interface for the S3 bucket.

The site's traffic is not as busy right now. Coupled with AWS Free Tier offering on this new AWS account, the operational cost per month for this setup is close to nil. So far the only costs involved are:

  - Annual fee for renewing the custom domain.

  - Monthly charges for the one and only administrator user on the business's G Suite account.

But for sure the cost will scale up slowly as the site's traffic picks up.

It is an exciting experience to explore building an entire website solely using AWS services. I enjoy spending time scouring through AWS documentations on S3, CloudFront, and ACM, understanding best practices and properly setting up least-privileged access in utilising those services.
