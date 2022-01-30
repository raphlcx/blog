---
title: "Kubernetes Ingress as load balancer"
date: 2022-01-30T00:48:14+08:00
---
In a [previous post on Kubernetes Service]({{< relref "kubernetes-service-and-kube-proxy" >}} "Kubernetes Service and kube-proxy"), we have explored using LoadBalancer-type for a service to route edge traffic to the service's virtual IP.

Now, imagine you run ten different back-end microservices internally. If each service uses a LoadBalancer-type service, your cloud provider will spin up ten load balancers, and each of them will incur hourly charges as they run. Not very forgiving for your wallet.

An alternative is to configure each service to use ClusterIP and write an Ingress manifest. An Ingress manifest contains ingress rules. An ingress rule specifies L7 routing rules, but it does nothing without an Ingress Controller.

An Ingress Controller is a Kubernetes application. It usually comes with a Kubernetes Service of type LoadBalancer. After deploying the Ingress Controller into a cluster, the cloud provider creates a load balancer for the controller's Kubernetes Service. The controller's pod, depending on implementation, contains a web server that reads all the Ingress objects in the cluster and sets up upstream for each of the hosts in the ingress rules.

For example, the popular Nginx Ingress Controller runs an Nginx web server pod in each Kubernetes worker node. It monitors Ingress objects within the cluster and creates an Nginx server block (`server {}`) to represent an upstream for each host that the ingress rules route.

So when traffic from the edge hits the load balancer:

  1. The load balancer, which belongs to the ingress controller, routes the traffic to the ingress controller's Kubernetes Service.

  1. From the service, it routes the traffic to its pods.

  1. Within the pods, the web servers, after consulting with its upstream configurations, route the traffic to the destined back-end microservice's Kubernetes Service ClusterIP.

  1. From there on, the microservice's ClusterIP will load balance the traffic to its pods' Endpoints.

Using Ingress and Ingress Controller allows us to pay for only a single load balancer on the cloud provider while keeping the microservices' Kubernetes Service as internal by using ClusterIP for them.
