---
title: Metadata and Metrics Collection
description: Learn how F5 NGINX Amplify Agent collects data.
weight: 200
toc: true
docs: DOCS-964
---

F5 NGINX Amplify Agent collects the following types of data, each serving specific purposes and use cases:

  * **NGINX metrics.** These metrics provide insights into the performance and health of NGINX servers. They are collected from [stub_status](http://nginx.org/en/docs/http/ngx_http_stub_status_module.html), the NGINX Plus status API, the NGINX log files, and the NGINX process state. Examples include request counts, active connections, and server response times. These metrics are crucial for monitoring server load and optimizing performance.
  * **System metrics.** These metrics describe the overall health and performance of the system hosting NGINX. They include CPU usage, memory usage, disk I/O, and network traffic. Monitoring these metrics helps in identifying resource bottlenecks and ensuring the system is running efficiently.
  * **PHP-FPM metrics.** These metrics are collected from the PHP-FPM pool status and include information such as the number of active processes, idle processes, and request processing times. They are essential for optimizing PHP application performance and ensuring efficient resource utilization.
  * **MySQL metrics.** These metrics are derived from the MySQL global status variables and include data on query performance, connection counts, and buffer usage. They are vital for database performance tuning and identifying potential issues in database operations.
  * **NGINX metadata.** This metadata provides detailed information about your NGINX instances, including package data, build information, the path to the binary, and build configuration options. It also includes NGINX configuration elements, which are crucial for auditing and compliance purposes. NGINX metadata also includes the NGINX configuration elements.
  * **System metadata.** This metadata includes basic information about the OS environment where NGINX Amplify Agent runs, such as the hostname, uptime, OS flavor, and other system details. It is useful for system inventory and management tasks. This can be the hostname, uptime, OS flavor, and other data.

NGINX Amplify Agent will mostly use Python's [psutil()](https://github.com/giampaolo/psutil) to collect the metrics, but occasionally it may also invoke certain system utilities like *ps(1)*.

While NGINX Amplify Agent is running on the host, it collects metrics at regular 20 second intervals. Metrics then get downsampled and sent to the Amplify backend once a minute.

Metadata is also reported every minute. Changes in the metadata can be examined through the Amplify web interface.

NGINX config updates are reported only when a configuration change is detected.

If NGINX Amplify Agent can't reach the Amplify backend to send the accumulated metrics, it will continue to collect metrics. Once the connectivity is re-established, it will send them over to Amplify. The maximum amount of data NGINX Amplify Agent can buffer is about 2 hours.
