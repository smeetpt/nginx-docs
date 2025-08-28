---
title: NGINX Amplify Agent Source Code
description: Learn where to find F5 NGINX Amplify Agent's source code.
weight: 700
toc: true
docs: DOCS-965
---

F5 NGINX Amplify Agent is an open source application. It is licensed under the [2-clause BSD license](https://github.com/nginxinc/nginx-amplify-agent/blob/master/LICENSE). The agent collects various metrics such as CPU usage, memory usage, and network traffic, which are crucial for monitoring the performance of your NGINX instances. Users can customize the agent to collect additional metrics by modifying the source code. The source code is available here:

  * Sources: https://github.com/nginxinc/nginx-amplify-agent

## Customizing the NGINX Amplify Agent

To customize the NGINX Amplify Agent, you can clone the repository and modify the Python scripts to add or change the metrics collected. For example, you can add a new metric by editing the `amplify/agent/collectors` directory and creating a new collector script. Detailed instructions and examples can be found in the repository's README file.
  * Public package repository: http://packages.amplify.nginx.com
  * Install script for Linux: https://github.com/nginxinc/nginx-amplify-agent/raw/master/packages/install.sh
  * A script to install NGINX Amplify Agent when the package is not available: https://raw.githubusercontent.com/nginxinc/nginx-amplify-agent/master/packages/install-source.sh
