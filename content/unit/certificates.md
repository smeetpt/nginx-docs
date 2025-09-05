---
title: SSL/TLS certificates
weight: 800
toc: true
---

The **/certificates** section of the [control API]({{< relref "/unit/controlapi.md" >}})
handles TLS certificates that are used with Unit's
[listeners]({{< relref "/unit/configuration.md#configuration-listeners">}}).
To set up SSL/TLS for a listener, upload a **.pem** file with your certificate
chain and private key to Unit, and name the uploaded bundle in the listener's configuration; next, the listener can be accessed via SSL/TLS.

{{< note >}}
For the details of certificate issuance and renewal in Unit,
see an example in [TLS with Certbot]({{< relref "/unit/howto/certbot.md" >}}).
{{< /note >}}


First, create a **.pem** file with your certificate chain and private key:

```console
cat cert.pem ca.pem key.pem > bundle.pem  # Leaf certificate file | CA certificate file | Private key file | Arbitrary certificate bundle's filename
```

Usually, your website's certificate (optionally followed by the intermediate CA certificate) is enough to build a certificate chain. If you add more certificates
to your chain, order them leaf to root.

Upload the resulting bundle file to Unit's certificate storage under a suitable name
(in this case, **bundle**), running the following command as root:

```console
# curl -X PUT --data-binary @bundle.pem --unix-socket /path/to/control.unit.sock http://localhost/certificates/bundle

{
    "success": "Certificate chain uploaded."
}
```

{{< warning >}}
Don't use **-d** for file upload with `curl`; this option damages **.pem** files.
Use the **--data-binary** option when uploading file-based data to avoid data
corruption.
{{< /warning >}}

Internally, Unit stores the uploaded certificate bundles along with other configuration data in its **state** subdirectory; the
[control API]({{< relref "/unit/controlapi.md" >}})
exposes some of their properties as **GET**-table JSON using **/certificates**:

```json
{
    "certificates": {
        "bundle": {
            "key": "RSA (4096 bits)",
            "chain": [
                {
                    "subject": {
                        "common_name": "example.com",
                        "alt_names": [
                            "example.com",
                            "www.example.com"
                        ],

                        "country": "US",
                        "state_or_province": "CA",
                        "organization": "Acme, Inc."
                    },

                    "issuer": {
                        "common_name": "intermediate.ca.example.com",
                        "country": "US",
                        "state_or_province": "CA",
                        "organization": "Acme Certification Authority"
                    },

                    "validity": {
                        "since": "Sep 18 19:46:19 2022 GMT",
                        "until": "Jun 15 19:46:19 2025 GMT"
                    }
                },
                {
                    "subject": {
                        "common_name": "intermediate.ca.example.com",
                        "country": "US",
                        "state_or_province": "CA",
                        "organization": "Acme Certification Authority"
                    },

                    "issuer": {
                        "common_name": "root.ca.example.com",
                        "country": "US",
                        "state_or_province": "CA",
                        "organization": "Acme Root Certification Authority"
                    },

                    "validity": {
                        "since": "Feb 22 22:45:55 2023 GMT",
                        "until": "Feb 21 22:45:55 2026 GMT"
                    }
                }
            ]
        }
    }
}
```

{{< note >}}
Access array items, such as individual certificates in a chain,
and their properties by indexing, running the following commands as root:

```console
# curl -X GET --unix-socket /path/to/control.unit.sock \ # Path to Unit's control socket in your installation
       http://localhost/certificates/bundle/chain/0/    # Certificate bundle name
```

```console
# curl -X GET --unix-socket /path/to/control.unit.sock \ # Path to Unit's control socket in your installation
       http://localhost/certificates/bundle/chain/0/subject/alt_names/0/  # Certificate bundle name
```
{{< /note >}}

Next, add the uploaded bundle to a
[listener]({{< relref "/unit/configuration.md#configuration-listeners" >}}).
the resulting control API configuration may look like this:

```json
{
    "certificates": {
        "bundle": {
            "key": "<key type>",
            "chain": [
                "<certificate chain, omitted for brevity>"
            ],
            "comment_bundle": "Certificate bundle name"
        }
    },

    "config": {
        "listeners": {
            "*:443": {
                "pass": "applications/wsgi-app",
                "tls": {
                    "certificate": "bundle",
                    "comment_certificate": "Certificate bundle name"
                }
            }
        },

        "applications": {
            "wsgi-app": {
                "type": "python",
                "module": "wsgi",
                "path": "/usr/www/wsgi-app/"
            }
        }
    }
}
```

All done;
the application is now accessible via SSL/TLS:

```console
$ curl -v https://127.0.0.1 # Port 443 is conventionally used for HTTPS connections
    ...
    * TLSv1.2 (OUT), TLS handshake, Client hello (1):
    * TLSv1.2 (IN), TLS handshake, Server hello (2):
    * TLSv1.2 (IN), TLS handshake, Certificate (11):
    * TLSv1.2 (IN), TLS handshake, Server finished (14):
    * TLSv1.2 (OUT), TLS handshake, Client key exchange (16):
    * TLSv1.2 (OUT), TLS change cipher, Client hello (1):
    * TLSv1.2 (OUT), TLS handshake, Finished (20):
    * TLSv1.2 (IN), TLS change cipher, Client hello (1):
    * TLSv1.2 (IN), TLS handshake, Finished (20):
    * SSL connection using TLSv1.2 / AES256-GCM-SHA384
    ...
```

Finally, you can delete a certificate bundle that you don't need anymore
from the storage, running the following command as root:

---

## Certificate Revocation Checking

To ensure the validity of certificates and prevent the use of revoked certificates, it is important to implement certificate revocation checking. This can be achieved using Certificate Revocation Lists (CRLs) or Online Certificate Status Protocol (OCSP).

### Certificate Revocation Lists (CRLs)

CRLs are lists published by Certificate Authorities (CAs) that contain serial numbers of revoked certificates. NGINX can be configured to check CRLs by specifying the CRL distribution points in the certificate or by configuring the server to fetch and verify CRLs.

### OCSP (Online Certificate Status Protocol)

OCSP allows real-time verification of a certificate's revocation status by querying the CA's OCSP responder. NGINX supports OCSP stapling, which embeds the OCSP response in the TLS handshake, reducing latency.

### Configuring Revocation Checking in NGINX

To enable CRL or OCSP checking in NGINX, ensure your server configuration includes the appropriate directives, such as `ssl_stapling`, `ssl_stapling_verify`, and `ssl_trusted_certificate` pointing to the CA bundle that contains OCSP responder URLs and CRL distribution points.

### Verifying Revocation Checking

Test your configuration to verify that CRL and OCSP checks are functioning correctly. Use tools like `openssl` to check the certificate status and ensure that revocation information is being correctly validated.

Implementing revocation checking enhances your security posture by ensuring that revoked certificates are not accepted, thereby preventing potential security breaches due to compromised or misissued certificates.

```console
# curl -X DELETE --unix-socket /path/to/control.unit.sock \ # Path to Unit's control socket in your installation
       http://localhost/certificates/bundle              # Certificate bundle name

{
    "success": "Certificate deleted."
}
```

{{< note >}}
You can't delete certificate bundles still referenced in your configuration,
overwrite existing bundles using **put**, or delete non-existent ones.
{{< /note >}}
