---
title: NGINX App Protect WAF Administration Guide
weight: 100
toc: true
type: how-to
product: NAP-WAF
docs: DOCS-1362
---

## Introduction

F5 NGINX App Protect WAF v5 is a Web Application Firewall (WAF) designed for both NGINX Open Source and NGINX Plus environments. It offers advanced security features and supports all the capabilities of [NGINX App Protect WAF v4]({{< ref "/nap-waf/v4/admin-guide/install.md" >}}). This solution is available as an add-on. It includes a dynamic NGINX module and containerized WAF services, providing strong security and scalability.

### Key Advantages

- Works with both NGINX Open Source and NGINX Plus.
- Scalable architecture for small and large deployments.
- Integrates smoothly with DevOps and SecOps workflows.

### Common Use Cases

- **E-Commerce Platforms:** Protects transactional data and user information in high-traffic environments.
- **API Protection:** Secures API gateways against common vulnerabilities.
- **DevOps Environments:** Integrates WAF into CI/CD pipelines to ensure security is part of the development lifecycle.

### Technical Requirements

- Basic knowledge of NGINX and containerization.
- Software prerequisites: Docker or Kubernetes, depending on your deployment method.

## Supported Operating Systems

NGINX App Protect WAF v5 supports these operating systems:

| Distribution   | Version(s)              |
| -------------- | ----------------------- |
| Alpine         | 3.19                    |
| Debian         | 11, 12                  |
| Ubuntu         | 20.04, 22.04, 24.04     |
| Amazon Linux   | 2023                    |
| RHEL           | 8, 9                    |
| Rocky Linux    | 8                       |
| Oracle Linux   | 8.1                     |

## Deployment Options

You can deploy NGINX App Protect WAF v5 in several ways:

1. **[Docker Compose Deployment]({{< ref "/nap-waf/v5/admin-guide/deploy-on-docker.md" >}}):**
   - Runs both NGINX and WAF components in containers.
   - Suitable for development, testing, and production.

2. **[Kubernetes Deployment]({{< ref "/nap-waf/v5/admin-guide/deploy-with-helm.md" >}}):**
   - Runs NGINX and WAF in a single pod.
   - Ideal for scalable, cloud-native environments.

3. **[NGINX on Host/VM with Containerized WAF]({{< ref "/nap-waf/v5/admin-guide/install.md" >}}):**
   - NGINX runs on the host or a virtual machine; WAF runs in containers.
   - Good for existing NGINX setups on hosts. Adding WAF does not disrupt your current NGINX configuration.

## NGINX App Protect WAF Compiler

NGINX App Protect WAF v5 uses a compiler to speed up deployment. The compiler converts security policies and logging profiles from JSON format into bundle files.

- Use the [NGINX App Protect WAF Compiler]({{< ref "/nap-waf/v5/admin-guide/compiler.md" >}}) to create these bundles.
- For signature updates, see the [Update App Protect Signatures]({{< ref "/nap-waf/v5/admin-guide/compiler.md#update-app-protect-signatures" >}}) section in the compiler documentation.

---

## Migrating from NGINX App Protect WAF v4 to v5

**Note:** Direct upgrades from v4 to v5 are not supported due to major architectural changes.

{{< note >}}
We recommend deploying NGINX App Protect WAF v5 in a staging environment first. Compile your policies with the WAF compiler and test enforcement before moving traffic from v4 to v5. Keep your v4 deployment as a backup during this process.
{{< /note >}}

Migration steps:

1. **Back up your configuration files.** This includes NGINX configs, JSON policies, logging profiles, user-defined signatures, and global settings.
2. **Install NGINX App Protect WAF v5.** Use either NGINX Open Source or NGINX Plus, depending on your application needs.
   - [Installing NGINX App Protect WAF]({{<ref "/nap-waf/v5/admin-guide/install.md">}})
   - [Deploying on Docker]({{<ref "/nap-waf/v5/admin-guide/deploy-on-docker.md">}})
   - [Deploying on Kubernetes]({{<ref "/nap-waf/v5/admin-guide/deploy-with-helm.md">}})
3. **Compile your `.json` policies and logging profiles into `.tgz` bundles** using the [compiler image]({{<ref "/nap-waf/v5/admin-guide/compiler.md">}}). Only compiled bundles are supported in v5.

   {{< note >}}
   If you previously used a default [logging profile]({{<ref "/nap-waf/v5/admin-guide/deploy-on-docker.md#using-policy-and-logging-profile-bundles">}}) JSON like `/opt/app_protect/share/defaults/log_all.json`, you can now use the constant `log_all` instead. In this case, you do not need to compile the logging profile into a bundle.

   ```nginx
   app_protect_security_log log_all /log_volume/security.log;
   ```
   {{< /note >}}

4. **Update your nginx.conf** to reference the new `.tgz` bundles instead of `.json` files. See [Using Policy and Logging Profile Bundles]({{<ref "/nap-waf/v5/admin-guide/install.md#using-policy-and-logging-profile-bundles">}}).
5. **Ensure the `.tgz` bundles are accessible** to the `waf-config-mgr` container.
6. **Restart your deployment** if it is already running. If you use the VM + containers deployment type, also restart NGINX. After migration, check the NGINX error log to confirm the process is running and there are no issues.


---

## Troubleshooting and FAQs

- For common deployment issues and solutions, see the [Troubleshooting Guide]({{< ref "/nap-waf/v5/troubleshooting-guide/troubleshooting.md#nginx-app-protect-5" >}}).
- Docker images for NGINX App Protect WAF v5 are built using Ubuntu 22.04 (Jammy) binaries.

---

## Contributing and Feedback

We welcome your feedback and contributions to improve this documentation!

- To suggest changes or report issues, you can [edit this page on GitHub](https://github.com/nginx/documentation/blob/main/content/nap-waf/v5/admin-guide/overview.md) or start a [Discussion](https://github.com/nginx/documentation/discussions).
- If you are new to Git or contributing, see our [contributing guidelines](https://github.com/nginx/documentation/blob/main/CONTRIBUTING.md) and [CONTRIBUTING_DOCS.md](https://github.com/nginx/documentation/blob/main/CONTRIBUTING_DOCS.md) for best practices.
- We appreciate all suggestions, corrections, and improvements!
