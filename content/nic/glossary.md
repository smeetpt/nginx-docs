---
description: null
docs: DOCS-1446
title: Glossary
weight: 10000
---

This is a comprehensive glossary of terms related to NGINX, including F5 NGINX Ingress Controller, Kubernetes, and other NGINX-related technologies.

---

## Ingress {#ingress}

_Ingress_ refers to an _Ingress Resource_, a Kubernetes API object which allows access to [Services](https://kubernetes.io/docs/concepts/services-networking/service/) within a cluster. They are managed by an [Ingress Controller]({{< ref "/nic/glossary.md#ingress-controller">}}).

_Ingress_ resources enable the following functionality:

- **Load balancing**, extended through the use of Services
- **Content-based routing**, using hosts and paths
- **TLS/SSL termination**, based on hostnames



---

## Ingress Controller {#ingress-controller}

*Ingress Controllers* are applications within a Kubernetes cluster that enable [Ingress]({{< ref "/nic/glossary.md#ingress">}}) resources to function. They are not automatically deployed with a Kubernetes cluster, and can vary in implementation based on intended use, such as load balancing algorithms for Ingress resources.


