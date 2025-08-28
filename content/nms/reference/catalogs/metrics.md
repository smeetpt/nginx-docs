---
description: Information about all of the Metrics collected by NGINX Agent
docs: DOCS-813
title: Metrics Catalog
toc: true
weight: 20
type:
- reference
---

## Metrics Collected by NGINX Agent

### Request Count
- **Description**: The total number of requests processed by the server.
- **Example**: If the server processes 1000 requests in a minute, the request count metric will reflect this number.
- **Use Case**: Useful for understanding the load on the server and for scaling decisions.

### Error Rate
- **Description**: The percentage of requests that result in an error.
- **Example**: If 5 out of 100 requests result in a 500 error, the error rate is 5%.
- **Use Case**: Helps in identifying issues with server performance or application errors.

### Response Time
- **Description**: The average time taken to respond to requests.
- **Example**: If the server takes an average of 200ms to respond, this metric will show that value.
- **Use Case**: Critical for performance tuning and ensuring a good user experience.

### Active Connections
- **Description**: The number of active connections to the server.
- **Example**: If there are 150 active connections at a given time, this metric will show that number.
- **Use Case**: Helps in monitoring server capacity and planning for scaling.

These are just a few examples of the metrics collected by the NGINX Agent. Each metric provides valuable insights into server performance and can be used to optimize and troubleshoot server operations.
