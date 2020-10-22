---
title: "Amazon S3 Block Public Access in layman"
date: 2020-09-21T01:17:43+08:00
tags: [aws,s3]
---
AWS has a feature for S3, known as Block Public Access, that allows us to easily prevent public access to an S3 bucket.

After reading and churning through its [official documentation](https://docs.aws.amazon.com/AmazonS3/latest/dev/access-control-block-public-access.html), I would like to provide a layman explaination for each setting in Block Public Access, when enabled.

  - `BlockPublicAcls`, prevents you from adding ACLs that grant public access to an S3 bucket.

  - `IgnorePublicAcls`, renders all buckets' ACLs that grant public access ineffective.

  - `BlockPublicPolicy`, prevents you from adding bucket policy that grants public access to an S3 bucket.

  - `RestrictPublicBuckets`, renders all buckets' policies that grant public access ineffective. If a bucket has policy that makes it public, it will also disable cross-account access to the bucket.
