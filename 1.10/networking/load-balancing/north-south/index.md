---
post_title: North-South Load Balancers
menu_order: 00
---

DC/OS Enterprise provides two north-south load balancing options out-of-the-box: Edge-LB and Marathon-LB.

# Edge-LB versus Marathon-LB

Edge-LB proxies and load balances traffic to _all_ services that run on DC/OS. In contrast, Marathon-LB can only load balance Marathon tasks. For example, if you are using Cassandra, Edge-LB can load balance the tasks launched by Cassandra. <!-- more is needed here. If I understand right, there's no real reason for an enterprise user to use Marathon-LB at all? --> Consider using Edge-LBif you need to load balance non-Marathon tasks, or a combination of Marathon and non-Marathon tasks. Consider Marathon-LB if you need to balance only Marathon tasks. <!-- this is sort of placeholder text because it's pretty obvious -->
