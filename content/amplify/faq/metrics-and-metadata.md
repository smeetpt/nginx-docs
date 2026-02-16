---
title: NGINX Amplify Metrics and Metadata
description: Questions about F5 NGINX Amplify's Metrics and Metadata
weight: 40
toc: true
docs: DOCS-957
---

### What Data Does F5 NGINX Amplify Agent Gather?

[NGINX Amplify Agent Metrics and Metadata]({{< ref "/amplify/nginx-amplify-agent/metadata-metrics-collection" >}})

The NGINX Amplify Agent collects a variety of metrics and metadata to help monitor and improve system performance and reliability. These include:

- **System Metrics**: CPU usage, memory usage, disk I/O, and network traffic. These metrics help in identifying resource bottlenecks and optimizing resource allocation.
- **NGINX Metrics**: Requests per second, active connections, and response times. These metrics are crucial for understanding the load on your server and ensuring efficient request handling.
- **Application Metrics**: Custom metrics that can be defined to monitor specific application parameters, providing insights into application performance.

Examples of how this data can be used include:
- **Performance Optimization**: By analyzing CPU and memory usage, you can identify and address performance bottlenecks.
- **Reliability Improvement**: Monitoring response times and active connections helps in maintaining service reliability and planning for scaling.

{{< note >}}For a complete list of metrics, refer to the [Metrics and Metadata documentation]({{< ref "/amplify/metrics-metadata" >}}).{{< /note >}}