---
post_title: Installing DC/OS
nav_title: Installing
menu_order: 030
---

DC/OS can be installed on any cluster of physical or virtual machines, on-premises or in the cloud.

To provide the best install experience possible, there are many different install methods tailored to specific environments.

The table below summarizes installation options.

| Environment | Node Type | Installer | Support Level | Docs |
|--------------------|--------------|-------------------|---------|---------------|
| Any | Any | Advanced Installer | Production | [docs](/docs/1.10/installing/advanced-installer/) |
| AWS | Virtual | Cloud Formation | Dev/Test | [docs](/docs/1.10/installing/cloud-templates/aws/) |
| Azure | Virtual | Azure Resource Manager | Dev/Test | [docs](/docs/1.10/installing/cloud-templates/azure/) |
| GCE | Virtual | Ansible | Dev/Test/Prod | [docs](/docs/1.10/installing/ansible/gce/) |
| Packet | Physical | Ansible | Dev/Test/Prod | [docs](/docs/1.10/installing/terraform/packet/) |
| DigitalOcean | Virtual | Terraform | Dev/Test/Prod | [docs](/docs/1.10/installing/terraform/digitalocean/) |
| Workstation | Virtual | DC/OS Vagrant | Dev/Test | [docs](/docs/1.10/installing/local/) |
| Laptop | Containers | DC/OS Docker | Dev/Test | [docs](/docs/1.10/installing/local/) |
