---
description: ''
docs: DOCS-1396
title: Glossary
toc: true
weight: 800
type:
- reference
---

This glossary defines terms used in the F5 NGINX One Console and F5 Distributed Cloud.


{{<bootstrap-table "table table-striped table-bordered">}}
| Term | Definition |
|------|------------|
| **Access Token** | (OIDC/OAuth2) A short-lived token that provides access to specific user resources as defined in the scope values in the request to the authorization server. May be a JSON token. |
| **Config Sync Group** | A group of NGINX systems (or instances) with identical configurations. They may also share the same certificates. Instances in a Config Sync Group could belong to different systems and even different clusters. For more information, see [Important considerations]({{< ref "/nginx-one/nginx-configs/config-sync-groups/manage-config-sync-groups.md#important-considerations" >}}). |
| **Data Plane** | The part of a network architecture that carries user traffic. In NGINX, the data plane is responsible for load balancing, caching, and serving web content. |
| **ID Token** | (OIDC) A JWT token that provides information about the authentication operation’s outcome. Used to convey identity information about the user. |
| **Identity Provider (IdP)** | (OIDC) A service that authenticates users and verifies their identity for client applications. |
| **Ingress** | (Kubernetes/NGINX Ingress Controller) A Kubernetes API object that allows access to [Services](https://kubernetes.io/docs/concepts/services-networking/service/) within a cluster. Managed by an Ingress Controller. Enables load balancing, content-based routing, and TLS/SSL termination. |
| **Ingress Controller** | (Kubernetes/NGINX Ingress Controller) An application within a Kubernetes cluster that enables Ingress resources to function. NGINX Ingress Controller is an implementation of this. |
| **Instance** | An individual system with NGINX installed. Instances can be grouped in a Config Sync Group. When adding an instance to NGINX One, a data plane key is required. |
| **JSON Web Token (JWT)** | An open standard (RFC 7519) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. Used for identity and access tokens in OIDC/OAuth2. |
| **Namespace** | (F5 Distributed Cloud/NGINX One) A grouping of a tenant’s configuration objects, similar to administrative domains. Ensures isolation and uniqueness within a tenant. Appears in the NGINX One Console URL as `/namespaces/<namespace name>/`. |
| **Protected Resource** | (OIDC/OAuth2) A resource hosted by the resource server that requires an access token to be accessed. |
| **Refresh Token** | (OIDC/OAuth2) A long-lived token used to obtain new access tokens. |
| **Relying Party (RP)** | (OIDC) A client service (such as NGINX Plus) that verifies user identity using an IdP. |
| **Staged Configurations** | Also known as **Staged Configs**. Allows you to save "work in progress" configurations. Can be created from scratch, from an Instance, another Staged Config, or a Config Sync Group. Does not have to be a working configuration until published. Can be managed via the [API]({{< ref "/nginx-one/api/api-reference-guide/#tag/StagedConfigs" >}}). |
| **Tenant** | (F5 Distributed Cloud/NGINX One) An entity that owns a specific set of configuration and infrastructure. Provides isolation; tenants cannot access each other's objects or infrastructure. Tenants can be individual or enterprise (with RBAC). |

<!-- Add more NGINX App Protect WAF terms here if/when available -->
{{</bootstrap-table>}}

**See also:** For broader industry and F5-specific terms, visit the [F5 Corporate Glossary](https://www.f5.com/glossary).

## Legal notice: Licensing agreements for NGINX products

Using NGINX One is subject to our End User Service Agreement (EUSA). For [NGINX Plus]({{< ref "/nginx" >}}), usage is governed by the End User License Agreement (EULA). Open source projects, including [NGINX Agent](https://github.com/nginx/agent) and [NGINX Open Source](https://github.com/nginx/nginx), are covered under their respective licenses. For more details on these licenses, follow the provided links.

---

## References

- [F5 Distributed Cloud: Core Concepts](https://docs.cloud.f5.com/docs/ves-concepts/core-concepts)
