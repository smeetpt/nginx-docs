---
title: Metadata and Metrics Collection
description: Learn how F5 NGINX Amplify Agent collects data.
weight: 200
toc: true
docs: DOCS-964
---

F5 NGINX Amplify Agent collects the following types of data:

  * **NGINX metrics.** NGINX Amplify Agent collects a lot of NGINX related metrics from [stub_status](http://nginx.org/en/docs/http/ngx_http_stub_status_module.html), the NGINX Plus status API, the NGINX log files, and from the NGINX process state. These metrics can be used to monitor request rates, error rates, and response times, helping to identify performance bottlenecks and optimize server configurations.
  * **System metrics.** These are various key metrics describing the system, for example, CPU usage, memory usage, network traffic, etc. Monitoring these metrics helps in understanding the resource utilization of the server, allowing for better capacity planning and ensuring that the system is not overburdened.
  * **PHP-FPM metrics.** NGINX Amplify Agent can obtain metrics from the PHP-FPM pool status if it detects a running PHP-FPM main process. These metrics are useful for monitoring the performance of PHP applications, such as tracking the number of active processes and request processing times, which can help in optimizing PHP-FPM configurations.
  * **MySQL metrics.** NGINX Amplify Agent can obtain metrics from the MySQL global status set of variables. These metrics can be used to monitor database performance, such as query execution times and connection statistics, aiding in database optimization and troubleshooting.
  * **NGINX metadata.** This is what describes your NGINX instances, and it includes package data, build information, the path to the binary, build configuration options, etc. NGINX metadata also includes the NGINX configuration elements. This information is crucial for auditing and ensuring that the server configurations are consistent with best practices.
  * **System metadata.** This is the basic information about the OS environment where NGINX Amplify Agent runs. This can be the hostname, uptime, OS flavor, and other data. Understanding the system environment helps in diagnosing issues related to compatibility and performance.

NGINX Amplify Agent will mostly use Python's [psutil()](https://github.com/giampaolo/psutil) to collect the metrics, but occasionally it may also invoke certain system utilities like *ps(1)*.

While NGINX Amplify Agent is running on the host, it collects metrics at regular 20 second intervals. Metrics then get downsampled and sent to the Amplify backend once a minute.

Metadata is also reported every minute. Changes in the metadata can be examined through the Amplify web interface.

NGINX config updates are reported only when a configuration change is detected.

If NGINX Amplify Agent can't reach the Amplify backend to send the accumulated metrics, it will continue to collect metrics. Once the connectivity is re-established, it will send them over to Amplify. The maximum amount of data NGINX Amplify Agent can buffer is about 2 hours.
