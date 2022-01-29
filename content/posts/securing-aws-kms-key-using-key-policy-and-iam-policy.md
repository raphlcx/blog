---
title: "Securing AWS KMS key using key policy and IAM policy"
date: 2020-09-12T16:28:53+08:00
tags: [aws]
---
AWS IAM policy contains the allowed actions for specified AWS resources. The policy is attached to a user, group, or role, allowing the principal to exercise the permissions defined in the policy. Apart from IAM policy, there is also a resource-based policy. This type of policy is attached directly to a resource instead of a principal.

For an AWS KMS key, the `kms:ViaService` condition key in the resource-based policy is worth a study.

Within the resource-based policy of an AWS KMS key with ARN `arn:aws:kms:us-east-1:111122223333:key/some-key`, we could specify the following in one of its policy statements:

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

When the IAM policy is attached to a user named John, John can utilise the KMS key, but only if he uses the KMS key for the AWS Secrets Manager service in North Virginia (us-east-1) region.

Using this approach, we can define *who* can use the KMS key via IAM policy and *what* the key can be used for via AWS KMS resource-based policy, therefore tightening the security control on the KMS key usage.

Note that if we specify the principal in the resource-based policy as `"AWS": "*"` instead, the IAM policy becomes redundant since everyone can use the KMS key, as long as it is for AWS Secrets Manager.
