---
title: "On AWS Config Custom Rule, and its friends"
date: 2020-10-22T20:38:20+08:00
tags: [aws]
---
I like exploring how different AWS products are designed and beautifully orchestrated to work together in symphony. This time, I'm embarking on this adventure with AWS Config, AWS Lambda, and AWS IAM.

AWS Config is a service intentionally designed for validating and ensuring governance compliance of an AWS account, regardless of whether it is compliance towards an enterprise-defined governance framework or a public governance framework such as NIST and PCI DSS. It keeps track of almost all the resources in the account, allowing us to run validation rules, i.e. AWS Config rules, to ensure our provisioned resources comply with the checks that the validation rule defines. For instance, we could use AWS Config rules to validate:

  - Whether any Amazon S3 buckets are publicly accessible.
  - Whether all our Redshift clusters are encrypted.
  - Whether provisioned EC2 instances are of a particular instance type.

AWS Config also ships with a set of pre-baked rules, known as AWS Config Managed Rules. If these rules do not fulfil our use case, we could write custom rules using AWS Lambda functions, which require more effort.

AWS Config triggers the relevant rules to evaluate whether the configuration changes on monitored resources cause compliance deviations. When an AWS Config custom rule is triggered, AWS Config calls the custom rule's Lambda function. This function then assumes its execution IAM role to fetch the necessary details from AWS Config, run the evaluation checks, and post the evaluation result to AWS Config.

And that is the flow on how AWS Config, AWS Lambda, and AWS IAM work together in delivering AWS Config custom rule as an end-user feature.
