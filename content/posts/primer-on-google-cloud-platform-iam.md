---
title: "Primer on Google Cloud Platform IAM"
date: 2021-11-02T22:26:51+08:00
---
Having worked with AWS IAM, I have always wanted to understand how other cloud providers design their IAM.

On Google Cloud Platform (GCP), RBAC is used extensively.

There are a few types of **principals**, i.e. users. These are primarily:

  - Google account, legit human user.
  - Service account, machine user.
  - Google group, well, a group of users.

**Resources** are any cloud resources that you create on GCP, for instance, a Compute Engine VM, a Pub/Sub topic, and etc.

Permissions are attached to **roles**, and never to principals. The permissions dictate the allowed actions on resources.

A principal is then associated with a role, and this role binding is called an **IAM policy**. Yes, it is confusing if you are coming from AWS.

In a linear flow:

  1. A principal, via IAM policy,
  2. is attached to a role, which, via permissions,
  3. decides permitted actions on resources.

There is a hierarchy in GCP:

```
Organisation
|
-- Folder
   |
   -- Project
      |
      -- Resources
```

If a role is attached to a principal on a higher level, the principal will retain the same permission throughout the lower-level entities and resources.

So, you can attach a Pub/Sub admin role to a principal at the project level, and this principal will have admin access to all the Pub/Sub resources in the same project.

There are two ways to restrict Pub/Sub admin access to only a subset of Pub/Sub resources:

  - Specify conditions on the role, i.e. limiting Pub/Sub admin access to only Pub/Sub topics with certain name.
  - Do not attach the Pub/Sub admin role at the project level, rather, attach the admin role on a particular Pub/Sub topic, at the resource level.

Yes, resources (not all types) can have its own specific role binding.

You can attach a very strict role to a principal on a higher level, and a permissive one on a specific resource. The effective permission on that resource is the union of the both.
