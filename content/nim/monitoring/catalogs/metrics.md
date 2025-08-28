---
description: Information about all of the Metrics collected by NGINX Agent
docs: DOCS-813
title: Metrics Catalog
toc: true
weight: 20
type:
- reference
---

## Detailed Metrics Information

### Metric 1: Request Count
- **Description**: The total number of requests processed by the server.
- **Example**: If the server processes 1000 requests in an hour, the request count metric will be 1000.
- **Use Cases**: Useful for understanding server load and scaling needs.

### Metric 2: Error Rate
- **Description**: The percentage of requests that result in an error.
- **Example**: If 5 out of 100 requests result in an error, the error rate is 5%.
- **Use Cases**: Helps in identifying issues with server performance or application errors.

### Metric 3: Response Time
- **Description**: The average time taken to respond to a request.
- **Example**: If the server takes an average of 200ms to respond, this metric will reflect that.
- **Use Cases**: Critical for performance tuning and ensuring a good user experience.

### Metric 4: Active Connections
- **Description**: The number of active connections to the server at any given time.
- **Example**: If there are 150 active connections, this metric will show 150.
- **Use Cases**: Important for monitoring server capacity and planning for scaling.

These examples provide a starting point for understanding the metrics collected by the NGINX Agent. Each metric can be further explored to tailor monitoring and performance optimization strategies.
