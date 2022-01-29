---
title: "Kubernetes Service and kube-proxy"
date: 2022-01-29T19:05:21+08:00
---
Kubernetes Service has several types (ClusterIP, NodePort, LoadBalancer), while kube-proxy can operate in different modes (userspace, iptables, IPVS). But how does network traffic flows from end to end, from the client to the backend pods, while navigating through Kubernetes Service and kube-proxy?

Kubernetes Service builds on top of one another. So if you use a LoadBalancer-type service, it comes with ClusterIP and NodePort (with certain exceptions depending on cloud providers). Kubernetes Service defines the components that handle traffic from the edge to its virtual, logical IP address:

  1. Edge traffic reaches the load balancer. The load balancer spreads the traffic to different nodes via NodePort.

  1. Each node receives the traffic via its NodePort, and forwards that to the service's virtual IP address, the ClusterIP.

  1. The ClusterIP receives the traffic, then, what now?

As we know, a Kubernetes Service's ClusterIP is only a virtual IP. It serves as a "gateway" for several Endpoint IP addresses belonging to the Kubernetes pods.

So when traffic reaches the ClusterIP, depending on kube-proxy operating mode:

  - When operating as a userspace proxy, an actual proxy process running in the node forwards the traffic to the running pods.

  - When operating in iptables mode, the kube-proxy installs iptables rules whenever the cluster creates a service. Traffic is routed on the kernel space to the pods via iptables rules without duplicating the traffic to userspace.

  - When operating in IPVS mode, it also utilises iptables rules, but with better performance and supports more load balancing algorithms.

Hence, kube-proxy defines the components that handle traffic from the virtual ClusterIP to the actual pods.
