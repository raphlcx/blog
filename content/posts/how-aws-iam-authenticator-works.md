---
title: "How aws-iam-authenticator works"
date: 2020-08-14T21:08:47+08:00
tags: [aws,kubernetes,authentication]
---
A few days ago I was reading up on [aws-iam-authenticator](https://github.com/kubernetes-sigs/aws-iam-authenticator). While trying to understand how it works, I came across this explanation for its client side component:

> We use this API in a somewhat unusual way by having the Authenticator client generate and pre-sign a request to the endpoint.

That really tickled my curiosity, and after reading its entire documentation, I constructed a mental model of its overall usage flow.

The flow of authenticating to AWS EKS using aws-iam-authenticator is as follow:

  1. A developer invokes aws-iam-authenticator.
  1. The authenticator's client component creates a HTTP request to be sent to AWS STS, and [signs this request](https://docs.aws.amazon.com/general/latest/gr/sigv4_signing.html) with the developer's AWS credentials. Note, the client only creates and signs this request, but does not send it to AWS STS.
  1. When attempting to connect to an AWS EKS cluster, the client sends this signed request as an authentication token.
  1. The API server of the EKS cluster is configured to use [Webhook Token Authentication](https://kubernetes.io/docs/reference/access-authn-authz/authentication/#webhook-token-authentication), where the remote webhook service is actually the authenticator's counterpart server component. The API server forwards this authentication token to the server component.
  1. Upon receiving the authentication token, the server component submits the token, which is actually a pre-signed request to AWS STS, to AWS STS.
  1. AWS STS verifies that the token or request is valid, and returns a response to the server component. The response includes the IAM identity of the developer who pre-signed the token beforehand.
  1. The server component, [with its role mapping](https://github.com/kubernetes-sigs/aws-iam-authenticator#full-configuration-format), maps the IAM identity to a Kubernetes user or role in the cluster.
  1. Now authenticated as a Kubernetes user or role, the developer now has access to the cluster.

When viewed as a whole, aws-iam-authenticator signs the request and submits it to AWS STS:

```
aws-iam-authenticator
  |
  v
AWS STS
```

But breaking it down:

```
aws-iam-authenticator [client]
  |
  v
AWS EKS API server
  |
  v
aws-iam-authenticator [server]
  |
  v
AWS STS
```

Analogically, it has the appearance of the client's token being stolen, and replayed by the server!

I have to say, it is a pretty clever trick.

All references to aws-iam-authenticator GitHub repository are taken as of commit [2c936a2](https://github.com/kubernetes-sigs/aws-iam-authenticator/commit/2c936a2108a739244311ac979eafaac2de142164).
