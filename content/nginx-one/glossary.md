---
description: ''
docs: DOCS-1396
title: Glossary
toc: true
weight: 800
type:
- reference
---

This is the unified glossary for all NGINX products, including NGINX Open Source, NGINX Plus, NGINX One, and NGINX Ingress Controller. For broader industry terms, see the [F5 Glossary](https://www.f5.com/glossary).

This glossary defines terms used in the F5 NGINX One Console and F5 Distributed Cloud.


{{<bootstrap-table "table table-striped table-bordered">}}
| Term        | Definition | Product(s) |
|-------------|-------------|-------------|
| **Config Sync Group** | A group of NGINX systems (or instances) with identical configurations. They may also share the same certificates. However, the instances in a Config Sync Group could belong to different systems and even different clusters. For more information, see this explanation of [Important considerations]({{< ref "/nginx-one/nginx-configs/config-sync-groups/manage-config-sync-groups.md#important-considerations" >}}) | NGINX One |
| **Data Plane** | The data plane is the part of a network architecture that carries user traffic. It handles tasks like forwarding data packets between devices and managing network communication. In the context of NGINX, the data plane is responsible for tasks such as load balancing, caching, and serving web content. | All |
| **Instance** | An instance is an individual system with NGINX installed. You can group the instances of your choice in a Config Sync Group. When you add an instance to NGINX One, you need to use a data plane key. | NGINX One |
| **Namespace** | In F5 Distributed Cloud, a namespace groups a tenant’s configuration objects, similar to administrative domains. Every object in a namespace must have a unique name, and each namespace must be unique to its tenant. This setup ensures isolation, preventing cross-referencing of objects between namespaces. You'll see the namespace in the NGINX One Console URL as `/namespaces/<namespace name>/` | NGINX One |
| **Staged Configurations** | Also known as **Staged Configs**. Allows you to save "work in progress." You can create it from scratch, an Instance, another Staged Config, or a Config Sync Group. It does _not_ have to be a working configuration until you publish it to an instance or a Config Sync Group. You can even manage your **Staged Configurations** through our [API]({{< ref "/nginx-one/api/api-reference-guide/#tag/StagedConfigs" >}}). | NGINX One |
| **Tenant** | A tenant in F5 Distributed Cloud is an entity that owns a specific set of configuration and infrastructure. It is fundamental for isolation, meaning a tenant cannot access objects or infrastructure of other tenants. Tenants can be either individual or enterprise, with the latter allowing multiple users with role-based access control (RBAC). | NGINX One |
| **Ingress** | Refers to an Ingress Resource, a Kubernetes API object that allows access to Services within a cluster. Enables load balancing, content-based routing, and TLS/SSL termination. | NGINX Ingress Controller |
| **Ingress Controller** | An application within a Kubernetes cluster that enables Ingress resources to function. NGINX Ingress Controller is an implementation of this. | NGINX Ingress Controller |
| **Identity Provider (IdP)** | A service that authenticates users and verifies their identity for client applications. | OIDC, NGINX Plus |
| **Protected Resource** | A resource hosted by the resource server that requires an access token to be accessed. | OIDC |
| **Relying Party (RP)** | A client service required to verify user identity. In OIDC, NGINX Plus acts as the RP. | OIDC, NGINX Plus |
| **JSON Web Token (JWT)** | An open standard (RFC 7519) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. | OIDC, NGINX Plus |
| **ID Token** | Specific to OIDC, a JWT that provides information about the authentication operation’s outcome. | OIDC |
| **Access Token** | Defined in OAuth2, this (optional) short lifetime token provides access to specific user resources as defined in the scope values in the request to the authorization server. | OIDC |
| **Refresh Token** | From OAuth2 specs, a long-lived token used to obtain new access tokens. | OIDC |

{{</bootstrap-table>}}

## Legal notice: Licensing agreements for NGINX products

Using NGINX One is subject to our End User Service Agreement (EUSA). For [NGINX Plus]({{< ref "/nginx" >}}), usage is governed by the End User License Agreement (EULA). Open source projects, including [NGINX Agent](https://github.com/nginx/agent) and [NGINX Open Source](https://github.com/nginx/nginx), are covered under their respective licenses. For more details on these licenses, follow the provided links.

---

## References

- [F5 Distributed Cloud: Core Concepts](https://docs.cloud.f5.com/docs/ves-concepts/core-concepts)
