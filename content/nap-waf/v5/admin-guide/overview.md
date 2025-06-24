---
title: NGINX App Protect WAF Administration Guide
weight: 100
toc: true
type: how-to
product: NAP-WAF
docs: DOCS-1362
---

<!--
Audit completed: 2024-06-24
- No screenshots present or required (conceptual/overview content)
- Readability, accessibility, and structure improved per #72
- No PII or sensitive data found
-->


## Introduction

F5 NGINX App Protect WAF v5 is a Web Application Firewall (WAF) for NGINX Open Source and NGINX Plus. It provides advanced security features and supports all the capabilities of [NGINX App Protect WAF v4]({{< ref "/nap-waf/v4/admin-guide/install.md" >}}).

This solution includes:
- A dynamic NGINX module
- Containerized WAF services

It is available as an add-on and is designed for robust security and scalability.

### Key Advantages

- Works with both NGINX Open Source and NGINX Plus
- Scalable for small and large deployments
- Integrates easily with DevOps and SecOps workflows

### Example Use Cases

- **E-Commerce Platforms**: Protect sensitive customer and transaction data in high-traffic environments.
- **API Gateways**: Secure APIs from common web vulnerabilities.
- **DevOps Pipelines**: Integrate WAF into CI/CD workflows to ensure security is part of the development process.

### Technical Requirements

- Basic knowledge of NGINX and containerization (see [NGINX documentation](https://docs.nginx.com/) and [Docker](https://docs.docker.com/) or [Kubernetes](https://kubernetes.io/docs/)).
- Software prerequisites:
  - Docker or Kubernetes, depending on your deployment method.

## Supported Operating Systems

NGINX App Protect WAF v5 can run on the following Linux distributions:

| Distribution | Version             |
| ------------ | ------------------- |
| Alpine       | 3.19                |
| Debian       | 11, 12              |
| Ubuntu       | 22.04, 24.04        |
| Amazon Linux | 2023                |
| RHEL         | 8, 9                |
| Rocky Linux  | 8, 9                |
| Oracle Linux | 8.1                 |

## Deployment Options

You can deploy NGINX App Protect WAF v5 in several ways:

1. **[Docker Compose Deployment]({{< ref "/nap-waf/v5/admin-guide/deploy-on-docker.md" >}})**
   - Runs both NGINX and WAF in containers
   - Good for development, testing, and production

2. **[Kubernetes Deployment]({{< ref "/nap-waf/v5/admin-guide/deploy-with-helm.md" >}})**
   - Runs NGINX and WAF together in a Kubernetes pod
   - Best for scalable, cloud-native environments

3. **[NGINX on Host/VM with Containerized WAF]({{< ref "/nap-waf/v5/admin-guide/install.md" >}})**
   - NGINX runs on the host or VM; WAF runs in containers
   - Useful if you already have NGINX running on your system; adding WAF does not disrupt your setup

## Policy and Logging Profile Compiler

NGINX App Protect WAF v5 uses a compiler to speed up deployment. This tool converts your security policies and logging profiles from JSON files into bundle files (for example, `.tgz`).

- Use the [NGINX App Protect WAF Compiler]({{< ref "/nap-waf/v5/admin-guide/compiler.md" >}}) to create these bundles.
- For updating security signatures, see the [Update App Protect Signatures]({{< ref "/nap-waf/v5/admin-guide/compiler.md#update-app-protect-signatures" >}}) section.


## Transitioning from NGINX App Protect WAF v4 to v5

> **Note:** Upgrading directly from v4 to v5 is not supported because of major architectural changes.

**Recommended migration steps:**

1. **Test in a staging environment first.**
   - Deploy NGINX App Protect WAF v5 in a test or staging environment.
   - Use the WAF compiler to compile your policies and test enforcement before moving production traffic.
   - Keep your v4 deployment as a backup until migration is complete.

2. **Back up your configuration files.**
   - Save your NGINX configuration, JSON policies, logging profiles, user-defined signatures, and global settings.

3. **Install NGINX App Protect WAF v5.**
   - Choose either NGINX Open Source or NGINX Plus, depending on your application needs.
   - See:
     - [Installing NGINX App Protect WAF]({{<ref "/nap-waf/v5/admin-guide/install.md">}})
     - [Deploying on Docker]({{<ref "/nap-waf/v5/admin-guide/deploy-on-docker.md">}})
     - [Deploying on Kubernetes]({{<ref "/nap-waf/v5/admin-guide/deploy-with-helm.md">}})

4. **Compile your policies and logging profiles.**
   - Use the [compiler image]({{<ref "/nap-waf/v5/admin-guide/compiler.md">}}) to convert `.json` files to `.tgz` bundles. Only compiled bundles are supported in v5.
   - If you used the default [logging profile]({{<ref "/nap-waf/v5/admin-guide/deploy-on-docker.md#using-policy-and-logging-profile-bundles">}}) JSON (like `/opt/app_protect/share/defaults/log_all.json`), you can now use the constant `log_all` instead. In this case, you do not need to compile the logging profile:

     ```nginx
     app_protect_security_log log_all /log_volume/security.log;
     ```

5. **Update your NGINX configuration.**
   - Replace `.json` references in `nginx.conf` with the new `.tgz` bundles. See [Using Policy and Logging Profile Bundles]({{<ref "/nap-waf/v5/admin-guide/install.md#using-policy-and-logging-profile-bundles">}}).

6. **Ensure bundle accessibility.**
   - Make sure the `.tgz` bundles are accessible to the `waf-config-mgr` container.

7. **Restart your deployment.**
   - If your deployment is already running, restart it. If you use the VM + containers method, also restart NGINX.
   - After migration, check the NGINX error log to confirm the process is running and there are no issues.


## Troubleshooting and FAQs

- For common deployment issues and solutions, see the [Troubleshooting Guide]({{< ref "/nap-waf/v5/troubleshooting-guide/troubleshooting.md#nginx-app-protect-5" >}}).
- Docker images for NGINX App Protect WAF v5 are built using Ubuntu 22.04 (Jammy) binaries.

---

## Before You Begin

- Make sure you have basic knowledge of NGINX and containerization.
- Choose your preferred deployment method (Docker, Kubernetes, or host/VM).
- Review the technical requirements and supported operating systems above.

## What's Next?

- [Install NGINX App Protect WAF]({{<ref "/nap-waf/v5/admin-guide/install.md">}})
- [Configure policies and logging profiles]({{<ref "/nap-waf/v5/admin-guide/compiler.md">}})
- [Troubleshooting Guide]({{< ref "/nap-waf/v5/troubleshooting-guide/troubleshooting.md#nginx-app-protect-5" >}})
- [More documentation](https://docs.nginx.com/)
