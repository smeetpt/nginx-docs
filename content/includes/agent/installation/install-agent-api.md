---
docs: DOCS-1031
files:
  - content/nim/nginx-app-protect/setup-waf-config-management.md
---

{{<note>}}Make sure `gpg` is installed on your system before continuing. You can install NGINX Agent using command-line tools like `curl` or `wget`.{{</note>}}

Before installing, you can validate the NGINX Instance Manager endpoint and its revocation status:

```bash
openssl s_client -connect <NIM_FQDN>:443 -servername <NIM_FQDN> -status | grep -E "Verify return code|OCSP response"
```

Note: OCSP status output appears only if the server staples OCSP. Absence of a stapled status is not itself an error, but you should address stapling and revocation checking as described in [Certificate revocation checking (CRL/OCSP)](https://docs.nginx.com/nginx-instance-manager/system-configuration/secure-traffic/#certificate-revocation-checking-crl-ocsp).

If your NGINX Instance Manager host doesn't use valid TLS certificates, you can use the insecure flags to bypass verification. Here are some example commands:

{{<tabs name="install-agent-api">}}

{{%tab name="curl"%}}

- **Secure:**

  ```bash
  curl https://<NIM_FQDN>/install/nginx-agent | sudo sh
  ```

- **Insecure:**

  ```bash
  curl --insecure https://<NIM_FQDN>/install/nginx-agent | sudo sh
  ```

{{<note>}}Caution: The --insecure flag disables certificate validation and certificate revocation checks (CRL/OCSP). Do not use this in production. Instead, use trusted CA bundles and, where possible, pin the issuing CA certificate rather than relying on broad system CA stores.{{</note>}}

To add the instance to a specific instance group during installation, use the `--instance-group` (or `-g`) flag:

```shell
curl https://<NIM_FQDN>/install/nginx-agent -o install.sh
chmod u+x install.sh
sudo ./install.sh --instance-group <instance group>
```

By default, the install script uses a secure connection to download packages. If it can’t establish one, it falls back to an insecure connection and logs this message:

```text
Warning: An insecure connection will be used during this nginx-agent installation
```

To enforce a secure connection, set the `--skip-verify` flag to false:

```shell
curl https://<NIM_FQDN>/install/nginx-agent -o install.sh
chmod u+x install.sh
sudo ./install.sh --skip-verify false
```

This enforces hostname and full certificate chain validation. For guidance on enforcing certificate revocation checking (CRL/OCSP) to avoid accepting revoked certificates—and on using trusted CA bundles or pinning the issuing CA—see [Certificate revocation checking (CRL/OCSP)](https://docs.nginx.com/nginx-instance-manager/system-configuration/secure-traffic/#certificate-revocation-checking-crl-ocsp).

{{%/tab%}}

{{%tab name="wget"%}}

- **Secure:**

  ```shell
  wget https://<NIM_FQDN>/install/nginx-agent -O - | sudo sh -s --skip-verify false
  ```

- **Insecure:**

  ```shell
  wget --no-check-certificate https://<NIM_FQDN>/install/nginx-agent -O - | sudo sh
  ```

{{<note>}}Caution: The --no-check-certificate flag disables certificate validation and certificate revocation checks (CRL/OCSP). Do not use this in production. Instead, use trusted CA bundles and, where possible, pin the issuing CA certificate rather than relying on broad system CA stores.{{</note>}}

To add your instance to a group during installation, use the `--instance-group` (or `-g`) flag:

```shell
wget https://<NIM_FQDN>/install/nginx-agent -O install.sh
chmod u+x install.sh
sudo ./install.sh --instance-group <instance group>
```

{{%/tab%}}

{{</tabs>}}
