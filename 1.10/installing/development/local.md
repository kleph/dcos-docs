---
post_title: Install DC/OS using a Local Installer
nav_title: Local Installer
menu_order: 3
---

DC/OS can be installed onto your workstation or laptop using one of two methods:

- [dcos-vagrant](https://github.com/dcos/dcos-vagrant/) uses Vagrant and VirtualBox and works on Mac, Windows, and Linux.
  This method best emulates a production environment but requires more local resources (CPU & Memory) to create VMs for each DC/OS node.
- [dcos-docker](https://github.com/dcos/dcos-docker/) uses Docker and works on Mac and Linux.
  This method requires fewer local resources and deploys faster by bypassing resource isolation and using containers instead of VMs.
