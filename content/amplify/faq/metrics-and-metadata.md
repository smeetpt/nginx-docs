---
title: NGINX Amplify Metrics and Metadata
description: Questions about F5 NGINX Amplify's Metrics and Metadata
weight: 40
toc: true
docs: DOCS-957
---

### What Data Does F5 NGINX Amplify Agent Gather?

[NGINX Amplify Agent Metrics and Metadata]({{< ref "/amplify/nginx-amplify-agent/metadata-metrics-collection" >}})

{{< note >}}For a complete list of metrics, refer to the [Metrics and Metadata documentation]({{< ref "/amplify/metrics-metadata" >}}).{{< /note >}}

### Definitions

- Data Collected by the Amplify Agent
  The Amplify Agent gathers a combination of metrics and metadata about NGINX, the host, and the agent itself. This includes:
  - NGINX metrics (requests, active connections, upstream status, etc.)
  - System metrics (CPU, memory, disk, network)
  - Agent metadata (agent version, host, timestamp)
  - Optional additional metrics as configured (see Instrumentation Guidance)

- What is Instrumented
  Instrumentation refers to the code paths and configuration that enable collection of these metrics and their propagation to the Amplify backend. Metrics and metadata are collected by the agent, parsed, and transmitted to Amplify for visualization. Instrumentation is designed to be low overhead and pluggable via configuration.

### Instrumentation Guidance

- Overview
  The Amplify Agent uses lightweight instrumentation to gather metrics and metadata from NGINX and the host. The instrumentation surface is exposed via the agent's config and is documented in the Instrumentation and Metrics Configuration docs.

- Enabling additional metrics
  To enable a broader set of metrics, follow the Instrumentation Guidance:
  - Refer to the Instrumentation Overview doc for the supported metric sets and their enabling flags.
  - Update the agent configuration with the appropriate flags or enablements, then restart the agent.

- Verifying in the Amplify UI
  After the agent restarts and begins sending data, open the Amplify UI and navigate to Metrics for your NGINX host. Verify that the newly enabled metrics appear under the NGINX metrics dashboards and correlate with expected signal within a few minutes.

- Important notes
  - Some metrics may require enabling specific modules or building optional instrumented components.
  - See the Metrics Configuration documentation for details on per-metric enablement and data retention.

### Practical Examples

- Example 1: Enable additional metrics and validate in UI
  1) Update agent config to enable additional metrics per the Instrumentation Guidance.
  2) Restart the Amplify Agent to apply changes.
  3) In the Amplify UI, navigate to Metrics > NGINX and confirm that the newly enabled metrics are visible and updating.
  4) Compare metric values against expected ranges during normal load to validate correctness.

- Example 2: Validate data presence after upgrade
  1) After upgrading Amplify components, confirm that the new metrics appear in the UI within 2-5 minutes.
  2) If missing, check agent logs for instrumentation errors and verify that the new metrics are enabled in the config.

- References
  - Instrumentation Overview: {{< ref "/amplify/instrumentation-overview" >}}
  - Metrics Configuration: {{< ref "/amplify/metrics-configuration" >}}