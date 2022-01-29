---
title: "Amazon S3 Block Public Access in layman"
date: 2020-09-21T01:17:43+08:00
tags: [aws]
---
Block Public Access is a feature on Amazon S3 that allows us to prevent public access to an S3 bucket easily.

After reading and churning through its [official documentation](https://docs.aws.amazon.com/AmazonS3/latest/dev/access-control-block-public-access.html), here is a layman explanation for each setting in Block Public Access, when enabled.

  - `BlockPublicAcls`. Prevents you from adding ACLs that grant public access to an S3 bucket.

  - `IgnorePublicAcls`. Renders all buckets' ACLs that grant public access ineffective.

  - `BlockPublicPolicy`. Prevents you from adding a bucket policy that grants public access to an S3 bucket.

  - `RestrictPublicBuckets`. Renders all buckets' policies that grant public access ineffective. If a bucket has a policy that makes it public, it will also disable cross-account access to the bucket.
