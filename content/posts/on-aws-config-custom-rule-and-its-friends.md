---
title: "On AWS Config Custom Rule, and its friends"
date: 2020-10-22T20:38:20+08:00
tags: [aws,aws-config]
---
I like exploring how different AWS products are designed and beautifully orchestrated to work together in symphony. This time, I'm embarking on this adventure with AWS Config, AWS Lambda, and AWS IAM.

AWS Config is a service intentionally designed for validating and ensuring governance compliance of an AWS account, regardless of whether it is compliance towards an enterprise-defined governance framework, or a public governance framework such as NIST and PCI DSS. It keeps track of pretty much all the resources that are created in the account, and allows us to run validation rules, known in the service as AWS Config rules, to ensure our provisioned resources comply with specified checks that we define in the rule. For instance, we could use AWS Config rules to validate:

  - Whether any Amazon S3 buckets are publicly accessible.
  - Whether all our Redshift clusters are encrypted.
  - Whether provisioned EC2 instances are of certain instance type.

Most of these checks are shipped with AWS's set of pre-baked rules, known as AWS Config Managed Rules. If the managed rules do not fulfil our use case, we could write custom rule using AWS Lambda functions. Using managed rules abstracts the details on the entire execution flow. The details surface once we start using custom rule.

Whenever there are configuration changes on the resources that are monitored by AWS Config, it triggers the relevant rules to execute, evaluating whether the change causes any compliance deviations. When an AWS Config custom rule is triggered, AWS Config calls the custom rule's Lambda function. This function then assumes its execution role, which is an AWS IAM role, to fetch the necessary details from AWS Config, run the evaluation checks, and when all is done, it posts the evaluation result back to AWS Config.

And that is the flow on how AWS Config, AWS Lambda, and AWS IAM work together in delivering AWS Config custom rule as an end-user feature.
