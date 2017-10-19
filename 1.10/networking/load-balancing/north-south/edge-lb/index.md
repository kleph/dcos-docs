---
post_title: Edge-LB
menu_order: 2
post_excerpt: ""
feature_maturity: ""
enterprise: 'yes'
---

Edge-LB proxies and load balances traffic to all services that run on DC/OS.

Edge-LB is built on HAProxy. HAProxy provides base functionality, such as load balancing for TCP and HTTP-based applications, SSL support, and health checking. On top of this, Edge-LB provides first class support for zero downtime service deployment strategies, such as blue/green deployment. Edge-LB subscribes to Mesos and updates HAProxy configuration in real time.
