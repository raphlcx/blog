---
title: "Two meanings of OIDC in AWS EKS"
date: 2021-08-31T00:39:42+08:00
---
As of 2021, AWS EKS has shipped two features that play well with OpenID Connect (OIDC). First is EKS as an OIDC identity provider, while another uses an external OIDC identity provider to authenticate into the cluster. Both mean different things.

## As an OIDC identity provider

Similar to how you would configure to use an external identity provider (IdP) to log in to AWS accounts. First, you log in on an IdP (Google, GitHub, Okta, etc.). Upon successful authentication, you gain access to the said AWS account.

As with the other IdPs, AWS EKS can act as an IdP itself. It isn't for authenticating a human user but more for streamlining the usage of AWS IAM roles on the workloads running inside the EKS cluster. Here's how it works:

1. Your app runs inside a Kubernetes pod. It needs to access an S3 bucket via an AWS IAM role.

1. The Kubernetes pod is first attached to a Kubernetes service account.

1. This service account is annotated with an AWS IAM role.

1. When the pod starts inside the cluster, the cluster detects the annotation on the service account. It then implicitly "authenticates" this service account, issuing a token and storing it in the pod.

1. The pod locates the token via an environment variable, `AWS_WEB_IDENTITY_TOKEN_FILE`, and uses this token to call AWS STS to assume its service account's annotated role. Upon authorisation, AWS STS issues temporary AWS credentials to the pod.

1. The pod, which now has the AWS credentials, gains access to the S3 bucket that it needs.

The AWS IAM role's trust relationship encodes the authorisation, which specifies it in this manner:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Federated": "arn:aws:iam::<ACCOUNT_ID>:oidc-provider/<OIDC_PROVIDER>"
      },
      "Action": "sts:AssumeRoleWithWebIdentity",
      "Condition": {
        "StringEquals": {
          "<OIDC_PROVIDER>:sub": "system:serviceaccount:<SERVICE_ACCOUNT_NAMESPACE>:<SERVICE_ACCOUNT_NAME>"
        }
      }
    }
  ]
}
```

The principal is a federated identity, i.e. an OIDC provider. The subject (user) is the service account annotated with the IAM role. That also explains why AWS terms this as IAM role for service account (IRSA).

## Authenticate to the cluster using OIDC IdP

On the contrary to the first OIDC feature that AWS EKS offers, this one is specifically to authenticate a human user.

Most users would interact with the Kubernetes API server via `kubectl`. Using this AWS EKS feature, you first authenticate with your external IdP, and upon successful authentication and authorisation with the cluster, `kubectl` returns you the response.

In a nutshell, this is how it works:

1. You invoke `kubectl` to retrieve information from your cluster.

1. Your client is redirected to the configured OIDC IdP (Google, GitHub, etc.).

1. You log in on the IdP, and your client receives an OIDC token upon successful authentication.

1. Your client is redirected back to the cluster's API server, carrying the token.

1. The cluster verifies the token, checks the authorisation, and returns you the response.

## Summary

So, two OIDC features on AWS EKS,

- Acts as an OIDC identity provider to AWS (specifically AWS STS). Allows your app to don an IAM role easily while running inside the cluster.
- Allows you (as a human) to authenticate using an external OIDC identity provider to gain access to the cluster.
