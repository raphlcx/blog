---
title: "Securing AWS KMS key using key policy and IAM policy"
date: 2020-09-12T16:28:53+08:00
tags: [aws,kms]
---
On AWS, we can define fine-grained permission policy on which user is allowed to manage and consume AWS resources, using IAM policy. IAM policy contains the allowed actions for specified AWS resources, and this policy is then attached to a user, group, or role, allowing the principal to exercise the permissions defined in the policy.

Let's take the case of AWS KMS key. I like to think of IAM policy as defining *who* can use which resources, e.g. which KMS key, but AWS KMS itself is integrated with many other AWS services, and without tightening the policy on what an KMS key can be used for, anyone who is authorised to use the resource could use it for any AWS services.

This is where AWS KMS condition keys in a key policy could help, particularly the `kms:ViaService` condition key.

On the key policy of an AWS KMS key with ARN `arn:aws:kms:us-east-1:111122223333:key/some-key`, we could specify the following in one of the policy statements:

```
{
  "Effect": "Allow",
  "Pricipal": {
    "AWS": "arn:aws:iam::111122223333:root"
  },
  "Actions": [
    "kms:Encrypt",
    "kms:Decrypt",
    "kms:GenerateDataKey*"
  ],
  "Resource": "*",
  "Condition": {
    "StringEquals": {
      "kms:ViaService": "secretsmanager.us-east-1.amazonaws.com",
      "kms:CallerAccount": "111122223333"
    }
  }
}
```

On the IAM policy that allows the consumption of the KMS key:

```
{
  "Effect": "Allow",
  "Actions": [
    "kms:Encrypt",
    "kms:Decrypt",
    "kms:GenerateDataKey*"
  ],
  "Resource": "arn:aws:kms:us-east-1:111122223333:key/some-key"
}
```

When the IAM policy is attached to a user named John, John will be able to utilise the KMS key, but only if the KMS key is used specifically for the AWS Secrets Manager service on North Virginia (us-east-1) region.

Using this approach, we are able to define *who* can use the KMS key via IAM policy, and *what* the key can be used for via AWS KMS key policy, therefore tightening the security control on the KMS key usage.

Note that if we specify the principal in the key policy as `"AWS": "*"` instead, IAM policy is essentially made redundant, since everyone is allowed to use the KMS key, as long as it is for AWS Secrets Manager.
