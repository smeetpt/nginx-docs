---
docs: "DOCS-1605"
---

This guide assumes that you have some familiarity with various Layer 7 (L7) Hypertext Transfer Protocol (HTTP) concepts, such as Uniform Resource Identifier (URI)/Uniform Resource Locator (URL), method, header, cookie, status code, request, response, and parameters.


For definitions of general NGINX and security terms, see the [Unified NGINX Glossary](https://docs.nginx.com/nginx-one/glossary).

The following terms are specific to NGINX App Protect WAF:

| Term | Definition |
| --- | --- |
| Alarm | If selected, the NGINX App Protect WAF system records requests that trigger the violation in the remote log (depending on the settings of the logging profile). |
| Attack signature | Textual patterns which can be applied to HTTP requests and/or responses by NGINX App Protect WAF to determine if traffic is malicious. For example, the string `<script>` inside an HTTP request triggers an attack signature violation. |
| Attack signature set | A collection of attack signatures designed for a specific purpose (such as Apache). |
| Bot signatures | Textual patterns which can be applied to an HTTP request's User Agent or URI by NGINX App Protect WAF to determine if traffic is coming from a browser or a bot (trusted, untrusted or malicious). For example, the string `googlebot` inside the User-Agent header will be classified as `trusted bot`, and the string `Bichoo Spider` will be classified as `malicious bot`. |
| Blocking response page | A blocking response page is displayed to a client when a request from that client has been blocked. Also called blocking page and response page. |
| Enforcement mode | Security policies can be in one of two enforcement modes: **Transparent mode** (Blocking is disabled; traffic is not blocked even if a violation is triggered) and **Blocking mode** (Blocking is enabled; traffic is blocked when a violation occurs if configured). |
| Loosening | The process of adapting a security policy to allow specific entities such as File Types, URLs, and Parameters. The term also applies to attack signatures, which can be manually disabled—effectively removing the signature from triggering any violations. |
| Tuning | Making manual changes to an existing security policy to reduce false positives and increase the policy’s security level. |