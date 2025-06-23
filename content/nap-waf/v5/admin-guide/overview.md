---
title: NGINX App Protect WAF Administration Guide
weight: 100
toc: true
type: how-to
product: NAP-WAF
docs: DOCS-1362
---

## Introduction

F5 NGINX App Protect WAF v5 is a Web Application Firewall (WAF) for NGINX Open Source and NGINX Plus. It protects your web applications from common threats and vulnerabilities. WAF v5 includes all features from [NGINX App Protect WAF v4]({{< ref "/nap-waf/v4/admin-guide/install.md" >}}), and adds new capabilities. This solution is available for purchase and includes a dynamic NGINX module and containerized WAF services. It is designed for strong security and easy scaling.

### Why Use NGINX App Protect WAF v5?

- Works with both NGINX Open Source and NGINX Plus.
- Scales easily for small or large deployments.
- Integrates smoothly with DevOps and SecOps workflows.

### Common Use Cases

- **E-Commerce**: Protects customer data and transactions in busy online stores.
- **API Security**: Shields API gateways from common attacks.
- **DevOps Pipelines**: Adds security checks to your development and deployment process.

### What You Need

- A basic understanding of NGINX and containers (like Docker or Kubernetes).
- Software: Docker or Kubernetes, depending on how you want to deploy.

## Supported Operating Systems

You can run NGINX App Protect WAF v5 on these operating systems:

| Distribution   | Version                  |
| -------------- | ------------------------ |
| Alpine         | 3.19                     |
| Debian         | 11, 12                   |
| Ubuntu         | 20.04, 22.04, 24.04      |
| Amazon Linux   | 2023                     |
| RHEL           | 8, 9                     |
| Rocky Linux    | 8                        |
| Oracle Linux   | 8.1                      |

## Deployment Options

You can deploy NGINX App Protect WAF v5 in several ways:

1. **[Docker Compose Deployment]({{< ref "/nap-waf/v5/admin-guide/deploy-on-docker.md" >}})**
   - Runs both NGINX and WAF in containers.
   - Good for development, testing, and production.

2. **[Kubernetes Deployment]({{< ref "/nap-waf/v5/admin-guide/deploy-with-helm.md" >}})**
   - Runs NGINX and WAF together in a Kubernetes pod.
   - Best for cloud-native, scalable setups.

3. **[NGINX on Host/VM with Containerized WAF]({{< ref "/nap-waf/v5/admin-guide/install.md" >}})**
   - NGINX runs on your server or virtual machine. WAF runs in containers.
   - Useful if you already have NGINX running and want to add WAF without changing your setup.

## Using the WAF Compiler

NGINX App Protect WAF v5 uses a compiler to speed up deployment. The compiler turns your security policies and logging profiles (in JSON format) into "bundle" files that the WAF can use quickly.

- Use the [NGINX App Protect WAF Compiler]({{< ref "/nap-waf/v5/admin-guide/compiler.md" >}}) to create these bundles.
- For updating security signatures, see the [Update App Protect Signatures]({{< ref "/nap-waf/v5/admin-guide/compiler.md#update-app-protect-signatures" >}}) section.

**Note:** A "bundle" is a compressed file (usually `.tgz`) that contains your compiled policies or logging profiles.

---

## Migrating from NGINX App Protect WAF v4 to v5

You cannot upgrade directly from v4 to v5 because the architecture has changed. Instead, follow these steps to move to v5:

{{< note >}}
**Tip:** Set up NGINX App Protect WAF v5 in a test (staging) environment first. Compile your policies, test them, and only then move your traffic from v4 to v5. Keep your v4 setup as a backup until you are sure everything works.
{{< /note >}}

**Migration Steps:**

1. **Back up your files.** Save your NGINX configs, JSON policies, logging profiles, custom signatures, and global settings.
2. **Install NGINX App Protect WAF v5.** Choose NGINX Open Source or NGINX Plus, depending on your needs.
   - [How to install NGINX App Protect WAF]({{<ref "/nap-waf/v5/admin-guide/install.md">}})
   - [How to deploy on Docker]({{<ref "/nap-waf/v5/admin-guide/deploy-on-docker.md">}})
   - [How to deploy on Kubernetes]({{<ref "/nap-waf/v5/admin-guide/deploy-with-helm.md">}})
3. **Compile your policies and logging profiles.** Use the [compiler-image]({{<ref "/nap-waf/v5/admin-guide/compiler.md">}}) to turn your `.json` files into `.tgz` bundles. WAF v5 only works with these compiled bundles.

   {{< note >}}
   If you used the default [logging profile]({{<ref "/nap-waf/v5/admin-guide/deploy-on-docker.md#using-policy-and-logging-profile-bundles">}}) like `/opt/app_protect/share/defaults/log_all.json`, you can now use the constant `log_all` instead. You do not need to compile this default profile:

   ```nginx
   app_protect_security_log log_all /log_volume/security.log;
   ```
   {{< /note >}}

4. **Update your nginx.conf.** Change any `.json` references to the new `.tgz` [bundles]({{<ref "/nap-waf/v5/admin-guide/install.md#using-policy-and-logging-profile-bundles">}}).
5. **Check file access.** Make sure the `.tgz` bundles are available to the `waf-config-mgr` container.
6. **Restart your deployment.** If you are using the VM + containers setup, restart NGINX too. After migration, check the NGINX error log to confirm everything is running smoothly.


---

## Troubleshooting and FAQs

If you run into problems, check the [Troubleshooting Guide]({{< ref "/nap-waf/v5/troubleshooting-guide/troubleshooting.md#nginx-app-protect-5" >}}) for help with common issues.

**Note:** Docker images for NGINX App Protect WAF v5 are built using Ubuntu 22.04 (Jammy).
