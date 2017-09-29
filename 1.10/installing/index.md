---
post_title: Installing and Upgrading DC/OS
nav_title: Installing and Upgrading
menu_order: 030
---

DC/OS can be installed on any cluster of physical or virtual machines.

# [Local Installation][1]

For first-time users or developers looking to build services or modify DC/OS, the [Vagrant installer][1] provides a quick, free way to deploy a virtual cluster on a single machine.

# [Cloud Provisioning][6]

For users deploying to the public cloud, DC/OS offers configurable cloud provisioning templates for [AWS][2] and [Azure][3] that will manage virtual machines and DC/OS installation.

DC/OS can be installed on other public and private clouds using the Datacenter Installation process, provided virtual machines and networks are created first.

# [Custom Installation][7]

For new users installing to existing virtual or physical machines, on-premises or in the cloud, the [GUI Installer][4] provides a guided experience for bootstrapping a DC/OS cluster.

For advanced users installing to existing virtual or physical machines, on-premises or in the cloud, the [Advanced Installer][5] provides a scriptable, automatable interface to integrate with your preferred configuration management system.

------------------

You can install DC/OS on bare metal, virtual machines and every cloud. With the custom installers, you have the flexibility to configure each installation of DC/OS exactly how you like it.

# GUI

You can use a simple graphical interface to configure and install DC/OS on your cluster. This method takes care of installing all the prerequisites on each instance along with everything required for DC/OS. Note that only a subset of the configuration options are available with this method. It is, however the fastest way to try out DC/OS on your own cluster.

- [GUI DC/OS Installation Guide][1]

# CLI

For those who'd like a little bit more opportunity to configure things, the CLI installer is a great choice. It can take care of installing all the system requirements, just like the GUI installer. The advantage is that you're able to pick more options as to exactly how your cluster is configured. An example of this would be the storage Exhibitor uses to bootstrap.

- [CLI DC/OS Installation Guide][2]

# Advanced

When you're ready to integrate DC/OS with your configuration management tools or create an image to roll out datacenter wide, the advanced installer is for you. This method provides you with all the flexibility you need to pick the actual installation process, every configuration option available and a simple way to consistently add agents to the cluster on a regular basis.

- [Advanced DC/OS Installation Guide][3]

[1]: gui/
[2]: cli/
[3]: advanced/

------------------


[1]: /docs/1.10/installing/local/
[2]: /docs/1.10/installing/cloud/aws/
[3]: /docs/1.10/installing/cloud/azure/
[4]: /docs/1.10/installing/custom/gui/
[5]: /docs/1.10/installing/custom/advanced/
[6]: /docs/1.10/installing/cloud/
[7]: /docs/1.10/installing/custom/
