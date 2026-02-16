---
description: Understanding how traffic metrics are collected, aggregated, and reported.
docs: DOCS-537
title: 'Overview: Traffic Metrics'
toc: true
weight: 100
type:
- concept
- reference
---

## Overview

The data that F5 NGINX Controller collects can be divided into two categories:

- **System metrics**: Data collected from the NGINX Plus API, the NGINX log files, and NGINX process state.
- **Traffic metrics**: Data related to processed traffic, with the ability to distinguish the Application, API endpoint, or Environment that traffic is directed through.

{{< note >}}
The key difference between system and traffic metrics is that traffic metrics are pre-aggregated for each time period.
{{< /note >}}

Metrics are published at a regular interval of 60 or 30 seconds for system and traffic metrics, respectively.

This topic gives an overview of the traffic metrics. Also known as "app-centric" metrics, traffic metrics contain information that lets you easily identify the App to which the data applies.

{{< see-also >}}
Refer to [View traffic metrics]({{< ref "/controller/analytics/metrics/view-traffic-metrics.md" >}}) for instructions on how to view traffic metrics using the [NGINX Controller REST API]({{< ref "/controller/api/_index.md" >}}).
{{< /see-also >}}

## Available traffic metrics

- `client.latency.{total | max | min | count}`: Measures the latency experienced by the client. Useful for identifying delays in client-server communication.
  - **Use Case**: Monitor client-side performance to ensure quick response times.
  - **Example**: High `client.latency.max` values may indicate network issues.

- `client.network.latency.{total | max | min | count}`: Captures the network latency from the client side.
  - **Use Case**: Diagnose network-related performance bottlenecks.
  - **Example**: Use `client.network.latency.total` to assess overall network delay.

- `client.request.latency.{total | max | min | count}`: Represents the time taken to process client requests.
  - **Use Case**: Optimize request handling to improve throughput.
  - **Example**: A high `client.request.latency.max` could suggest server processing delays.

- `client.ttfb.latency.{total | max | min | count}`: Time to first byte latency, indicating server responsiveness.
  - **Use Case**: Ensure server is responsive to initial client requests.
  - **Example**: High `client.ttfb.latency.max` may indicate server-side processing delays.

- `client.response.latency.{total | max | min | count}`: Measures the latency in sending responses back to the client.
  - **Use Case**: Identify slow response times affecting user experience.
  - **Example**: Use `client.response.latency.total` to evaluate response efficiency.

- `upstream.network.latency.{total | max | min | count}`: Network latency experienced by upstream servers.
  - **Use Case**: Monitor upstream server performance and network conditions.
  - **Example**: High `upstream.network.latency.max` could indicate upstream network issues.

- `upstream.header.latency.{total | max | min | count}`: Latency in processing upstream headers.
  - **Use Case**: Optimize header processing to reduce delays.
  - **Example**: Analyze `upstream.header.latency.total` for header processing efficiency.

- `upstream.response.latency.{total | max | min | count}`: Latency in receiving responses from upstream servers.
  - **Use Case**: Ensure timely responses from upstream services.
  - **Example**: High `upstream.response.latency.max` may indicate slow upstream responses.

- `http.request.bytes_rcvd`: Total bytes received in HTTP requests.
  - **Use Case**: Monitor data inflow to manage bandwidth usage.
  - **Example**: Sudden spikes in `http.request.bytes_rcvd` could indicate increased traffic.

- `http.request.bytes_sent`: Total bytes sent in HTTP requests.
  - **Use Case**: Track data outflow to optimize bandwidth allocation.
  - **Example**: Use `http.request.bytes_sent` to assess outgoing data volume.

- `http.request.count`: Number of HTTP requests processed.
  - **Use Case**: Measure application load and request handling capacity.
  - **Example**: High `http.request.count` values may require scaling resources.

{{< see-also >}}
Refer to the [NGINX Controller Metrics Catalog]({{< ref "/controller/analytics/catalogs/metrics.md" >}}) for details about these and the other metrics that NGINX Controller reports.
{{< /see-also>}}

## Calculating traffic metrics

As traffic flows through a configured application, NGINX Controller collects the traffic-related data. With heavy traffic, the number of single, distinguishable metrics can be challenging to discern. For this reason, the metric values are aggregated.

The aggregation happens every publish period -- this period is stored in the `aggregation_duration` dimension, and is usually 30 seconds -- and is based on metric dimensions.

Metrics are aggregated using four aggregation functions:

- **SUM** for `http.request.bytes_rcvd`, `http.request.bytes_sent` and all metrics with `.total` suffix.
- **MAX** for metrics with `.max` suffix.
- **MIN** for metrics with `.min` suffix.
- **COUNT** for metrics with `.count` suffix.

### Example

To better understand how metrics are aggregated, consider the following example:

Imagine you have one application configured with one URI (recorded in the `http.uri` dimension of each traffic-related metric). In the last 30 seconds, a user queried that URI five times. The `client.request.latency` values for the requests were: 1 ms, 2 ms, 3 ms, 4 ms, and 5 ms.

The final metric values returned by the Metrics API will be:

- `http.request.count` = 5
- `client.request.latency.total` = 15 ms
- `client.request.latency.max` = 5 ms
- `client.request.latency.min` = 1 ms
- `client.request.latency.count` = 5

{{< versions "3.0" "latest" "ctrlvers" >}}
{{< versions "3.18" "latest" "apimvers" >}}
{{< versions "3.20" "latest" "adcvers" >}}
