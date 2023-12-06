---
title: "Visualising the pieces in AWS SSO"
date: 2023-12-07T00:04:11+08:00
---

Hey, I want to share a simplisitc view of how AWS SSO group, permission set, and account assignment work together using a simple illustration.

```
  [AWS SSO]                            [AWS IAM]
o------------------------------o     o----------------o
|                              |     |                |
|  o-----o   o--------------o  |     |  o----------o  |
|  |group|   |permission set|<----------|IAM policy|  |
|  o-----o   o--------------o  |     |  o----------o  |
|     |             |          |     |                |
|     ---------------          |     o----------------o
|            |                 |
|            v                 |
|   o------------------o       |
|   |account assignment|       |
|   o------------------o       |
|            ^                 |
o------------|-----------------o
             |
       [AWS account]
```

With an account assignment, a group (i.e. entity) can access an AWS account with a specified permission set, where the permission set is backed by AWS IAM policies.
