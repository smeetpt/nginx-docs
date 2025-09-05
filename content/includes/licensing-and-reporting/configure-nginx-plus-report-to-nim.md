---
docs:
---

1. Open port `443` for NGINX Instance Manager.

2. On each NGINX Plus instance, update the [`usage_report`](https://nginx.org/en/docs/ngx_mgmt_module.html#usage_report) directive in the [`mgmt`](https://nginx.org/en/docs/ngx_mgmt_module.html) block of the NGINX configuration (`/etc/nginx/nginx.conf`) to point to your NGINX Instance Manager host:

    ```nginx
    mgmt {
      usage_report endpoint=<NGINX-INSTANCE-MANAGER-FQDN>;
    }
    ```

    {{<call-out "note" "Extra steps for self-signed certificates">}}If you use self-signed certificates in your NGINX Instance Manager environment, follow the steps in [Configure SSL verification for usage reporting with self-signed certificates]({{< ref "nim/system-configuration/secure-traffic.md#configure-ssl-verify" >}}).{{</call-out>}}

    {{<call-out "note" "Revocation and the usage report connection">}}The `usage_report` connection is an outbound HTTPS client from NGINX to NGINX Instance Manager. Ensure the NIM endpoint presents a valid, non-revoked server certificate issued by a trusted CA. For production, avoid self-signed certificates; if you must use a private CA or self-signed certificate, import the issuing CA into the NGINX host's trust store and use short-lived server certificates with automated rotation. If your security policy requires active revocation checks, front NGINX Instance Manager with an NGINX server that staples OCSP (`ssl_stapling on;` `ssl_stapling_verify on;`) so monitoring can detect issues quickly. CRL/OCSP configuration for the `usage_report` HTTPS client is not exposed here; see [Certificate revocation checking (CRL/OCSP)]({{< ref "nim/system-configuration/secure-traffic.md#certificate-revocation-checking-crl-ocsp" >}}) for server-side hardening and operational guidance.{{</call-out>}}

3. Reload NGINX:

    ``` bash
    systemctl reload nginx
    ```
