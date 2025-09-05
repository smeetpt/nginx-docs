---
title: Encrypt communication
toc: true
weight: 200
docs: DOCS-802
---

## Overview

Follow the steps in this guide to encrypt communication between NGINX Agent and Instance Manager with TLS.

## Before You Begin

To enable mTLS, you must have TLS enabled and supply a key, cert, and a CA cert on both the client and server. See the [Secure Traffic with Certificates](https://docs.nginx.com/nginx-instance-manager/system-configuration/secure-traffic/) topic for instructions on how to generate keys and set them in the specific values in the NGINX Agent configuration.

## Enabling mTLS

See the examples below for how to set these values using a configuration file, CLI flags, or environment variables.

### Enabling mTLS via Config Values

You can edit the `/etc/nginx-agent/nginx-agent.conf` file to enable mTLS for NGINX Agent. Make the following changes:

```yaml
server:
  metrics: "cert-sni-name"
  command: "cert-sni-name"
tls:
  enable: true
  cert: "path-to-cert"
  key: "path-to-key"
  ca: "path-to-ca-cert"
  skip_verify: false
```

The `cert-sni-name` value should match the SubjectAltName of the server certificate. For more information see [Configuring HTTPS servers](http://nginx.org/en/docs/http/configuring_https_servers.html).

### Enabling mTLS with CLI Flags

To enable mTLS for the NGINX Agent from the command line, run the following command:

```bash
nginx-agent --tls-cert "path-to-cert" --tls-key "path-to-key" --tls-ca "path-to-ca-cert" --tls-enable
```

### Enabling mTLS with Environment Variables

To enable mTLS for NGINX Agent using environment variables, run the following commands:

```bash
NGINX_AGENT_TLS_CA="my-env-ca"
NGINX_AGENT_TLS_KEY="my-env-key"
NGINX_AGENT_TLS_CERT="my-env-cert"
NGINX_AGENT_TLS_ENABLE=true
```

<br>

---

## Certificate revocation checking (CRL/OCSP)

### Overview
Certificate revocation ensures that certificates that are compromised, mis-issued, or otherwise invalid are rejected. For NGINX Instance Manager (NIM) and NGINX Agent mTLS, you should:
- Enforce revocation of Agent client certificates on the NIM side using Certificate Revocation Lists (CRLs).
- Enable OCSP stapling on the NIM TLS endpoint so clients receive the server certificate's status with the TLS handshake.

Current limitations:
- NGINX does not verify client certificates via OCSP. Use CRLs to enforce revocation for client (Agent) certificates.
- NGINX Agent does not perform OCSP/CRL checks of the server certificate. Rely on strict verification (tls.skip_verify: false), CA pinning, short‑lived certificates with automated rotation, and server-side OCSP stapling for better hygiene and monitoring.

### Prerequisites
- Accurate system time (NTP) on all nodes participating in TLS.
- Access to CRL files from your issuing CA for Agent client certificates.
- If enabling OCSP stapling on the NIM endpoint: allow egress from NIM to CA OCSP responders, have the full certificate chain available, and configure a DNS resolver.
- Avoid skip_verify: true in production; it bypasses chain, name, and revocation checks.

### NIM validates Agent client certificates (mTLS) using CRLs
Place NIM behind NGINX to terminate TLS and validate Agent client certificates. Configure NGINX to require client certs and enforce revocation via CRLs:

```nginx
server {
  listen 443 ssl;
  server_name nim.example.com;

  # Server cert/key for NIM endpoint
  ssl_certificate     /etc/nginx/nim/server.crt;
  ssl_certificate_key /etc/nginx/nim/server.key;

  # Verify Agent (client) certificates
  ssl_client_certificate /etc/nginx/nim/ca/agent-issuing-ca.pem;
  ssl_verify_client on;  # require client certs
  ssl_verify_depth 2;

  # Enforce revocation of client certs via CRL
  ssl_crl /etc/nginx/nim/ca/agent-issuing-ca.crl.pem;  # PEM-encoded CRL

  # Harden server-side presentation to clients (see next section)
  ssl_stapling on;
  ssl_stapling_verify on;
  ssl_trusted_certificate /etc/nginx/nim/ca/fullchain.pem;  # for OCSP responder trust
  resolver 1.1.1.1 8.8.8.8 valid=300s;
  resolver_timeout 5s;

  location / {
    proxy_pass http://nim-backend:8035;  # adjust to your NIM service
  }
}
```

Notes:
- Convert DER CRLs to PEM if needed: openssl crl -in ca.crl -inform DER -out ca.crl.pem -outform PEM
- Update CRLs regularly according to your CA's publishing schedule and reload NGINX. Stale CRLs may allow revoked certs.
- Keep CRLs small/manageable by issuing Agent certs from an intermediate CA dedicated to Agents.
- NGINX does not perform OCSP checks for client certificates; use ssl_crl for mTLS client revocation enforcement. Ensure the CRL corresponds to the issuing CA for Agent certificates.

### Agent validates the NIM server certificate (client-side)
- Keep tls.skip_verify set to false (default) so hostname and chain validation are enforced.
- Pin your issuing CA by setting tls.ca to a minimal bundle that includes only the CA(s) that issue the NIM server certificate. Avoid broad system bundles.
- Prefer short-lived server certificates and automate rotation; promptly revoke compromised certs.
- Enable OCSP stapling on the NIM TLS endpoint (as above). The Agent does not itself validate OCSP/CRL, but stapling improves overall security posture and observability.

Agent configuration recap:

```bash
tls:
  enable: true
  cert: "/path/to/agent-cert.pem"           # if mTLS
  key: "/path/to/agent-key.pem"             # if mTLS
  ca: "/etc/pki/org-ca/issuing-ca.pem"      # pin your issuing CA
  skip_verify: false
```

### Cautions
- Setting skip_verify: true disables hostname and chain checks and can lead to trusting revoked/forged server certificates.
- Stale or missing CRLs mean revoked Agent client certificates may still be accepted. Automate CRL refresh and nginx reload.
- Treat OCSP stapling failures as an operational alert; monitor error logs for ocsp errors and investigate promptly.

### Verification
- Verify OCSP stapling from the NIM endpoint:
  openssl s_client -connect nim.example.com:443 -status -servername nim.example.com | grep -A3 "OCSP response"
- Verify a revoked Agent client certificate is rejected (expect TLS handshake failure and a 400/495 status at the edge): attempt client auth with a revoked cert and confirm access is denied.

See also:
- Enabling mTLS (above) for how to enable TLS/mTLS on the Agent.
- Secure Traffic with Certificates for key/cert generation and configuration guidance: https://docs.nginx.com/nginx-instance-manager/system-configuration/secure-traffic/
- Insecure Mode (Not Recommended) (below) for the risks of disabling TLS verification.

---

## Enabling Server-Side TLS

To enable server-side TLS you must have TLS enabled. See the following examples for how to set these values using a configuration file, CLI flags, or environment variables.

### Enabling Server-Side TLS via Config Values

You can edit the `/etc/nginx-agent/nginx-agent.conf` file to enable server-side TLS. Make the following changes:

```bash
tls:
  enable: true
  skip_verify: false
```

### Enabling Server Side TLS with CLI Flags

To enable server-side TLS from the command line, run the following command:

```bash
nginx-agent --tls-enable
```

### Enabling Server-Side TLS with Environment Variables

To enable server-side TLS using environment variables, run the following commands:

```bash
NGINX_AGENT_TLS_ENABLE=true
```

<br>

---

## Enable Server-Side TLS With Self-Signed Certificate

{{< warning >}}These steps are not recommended for production environments.{{< /warning >}}

To enable server-side TLS with a self-signed certificate, you must have TLS enabled and set `skip_verify` to `true`, which disables hostname validation. Setting `skip_verify` can be done done only by updating the configuration file. See the following example:

```bash
tls:
  enable: true
  skip_verify: true
```

## Insecure Mode (Not Recommended)

To enable insecure mode, you simply need to set `tls:enable` to `false`. Setting this value to `false` can be done only by updating the configuration file or with environment variables. See the following examples:

### Enabling Insecure Mode via Config Values

You can edit the `/etc/nginx-agent/nginx-agent.conf` file to enable insecure mode. Make the following changes:

```bash
tls:
  enable: false
```

### Enabling Insecure Mode with Environment Variables

To enable insecure mode using environment variables, run the following commands:

```bash
NGINX_AGENT_TLS_ENABLE=false
```
