---
title: NGINX Amplify Agent Source Code
description: Learn where to find F5 NGINX Amplify Agent's source code.
weight: 700
toc: true
docs: DOCS-965
---

F5 NGINX Amplify Agent is an open source application designed to monitor the performance and health of NGINX instances. It collects various metrics such as CPU, memory usage, and network traffic, providing insights into the server's performance. The agent is particularly useful for identifying bottlenecks and optimizing server configurations. It is licensed under the [2-clause BSD license](https://github.com/nginxinc/nginx-amplify-agent/blob/master/LICENSE), and is available here:

  * Sources: https://github.com/nginxinc/nginx-amplify-agent

## Utilizing the Source Code
The source code of the NGINX Amplify Agent can be modified to suit specific monitoring needs. Developers can add custom metrics or integrate the agent with other monitoring tools. The open-source nature of the agent allows for extensive customization and enhancement to fit various deployment scenarios.
  * Public package repository: http://packages.amplify.nginx.com
  * Install script for Linux: https://github.com/nginxinc/nginx-amplify-agent/raw/master/packages/install.sh
  * A script to install NGINX Amplify Agent when the package is not available: https://raw.githubusercontent.com/nginxinc/nginx-amplify-agent/master/packages/install-source.sh
