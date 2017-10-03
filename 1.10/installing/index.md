---
post_title: Installing DC/OS
nav_title: Installing
menu_order: 030
---

DC/OS can be installed on any cluster of physical or virtual machines, on-premises or in the cloud.

To provide the best install experience possible, there are many different install methods tailored to specific environments.

# Installers

| Environment | Node Type | Installer | Support Level | Docs |
|-------------|-----------|-----------------|---------------|------|
| Any | Any | Advanced Installer | Production | [docs](/docs/1.10/installing/advanced-installer/) |
| Any | Any | CLI Installer | Dev/Test | [docs](/docs/1.10/installing/cli-installer/) |
| Any | Any | GUI Installer | Dev/Test | [docs](/docs/1.10/installing/gui-installer/) |

# Cloud templates

| Environment | Node Type | Template Engine | Support Level | Docs |
|-------------|-----------|-----------------|---------------|------|
| AWS | Virtual | Cloud Formation | Dev/Test | [docs](/docs/1.10/installing/cloud-templates/aws/) |
| Azure | Virtual | Azure Resource Manager | Dev/Test | [docs](/docs/1.10/installing/cloud-templates/azure/) |

# Provisioners

| Environment | Node Type | Provisioner | Project | Support Level | Docs |
|-------------|-----------|-------------|---------|---------------|------|
| GCE | Virtual | Ansible | [dcos-gce](https://github.com/dcos-labs/dcos-gce) | Community | [docs](/docs/1.10/installing/ansible/gce/) |
| AWS | Virtual | Ansible | [ansible-dcos](https://github.com/vishnudxb/ansible-dcos) | Community | N/A |
| AWS | Virtual | Ansible | [ansible-role-dcos](https://github.com/mGageTechOps/ansible-role-dcos) | Community | N/A |
| AWS | Virtual | Ansible | [dcos-ansible](https://github.com/kbokh/dcos-ansible) | Community | N/A |
| AWS/Azure | Virtual | Terraform | [bernadinm/terraform-dcos](https://github.com/bernadinm/terraform-dcos) | Community | N/A |
| AWS | Virtual | Terraform | [zutherb/terraform-dcos](https://github.com/zutherb/terraform-dcos) | Community | N/A |
| AWS | Virtual | Terraform | [dcos-up](https://github.com/kensuio/dcos-up) | Community | N/A |
| DigitalOcean | Virtual | Terraform | [digitalocean-dcos-terraform](https://github.com/jmarhee/digitalocean-dcos-terraform) | Community | [docs](/docs/1.10/installing/terraform/digitalocean/) |
| Packet | Physical | Terraform | [packet-terraform](https://github.com/dcos/packet-terraform) | Community | [docs](/docs/1.10/installing/terraform/packet/) |

# Local provisioners

| Environment | Node Type | Provisioner | Project | Support Level | Docs |
|-------------|-----------|-------------|---------|---------------|------|
| Workstation | Virtual | Vagrant | [dcos-vagrant](https://github.com/dcos/dcos-vagrant) | Dev/Test | [docs](/docs/1.10/installing/local/) |
| Laptop | Containers | Docker | [dcos-docker](https://github.com/dcos/dcos-docker) | Dev/Test | [docs](/docs/1.10/installing/local/) |

# Install method attributes

- **Environment** - DC/OS can be installed on any cloud: hosted or on-premises.
- **Node Type** - DC/OS can be installed on virtual machines, physical machines, or even a cluster of containers.
- **Installer** - DC/OS has one primary Advanced installer with a CLI and GUI wrapper for ease of use.
- **Template Engine** - Cloud templates can make DC/OS easier to install with cloud-specific features and optimizations.
- **Provisioner** - The DC/OS Advanced installer may be automated with your favorite provisioner to combine installation with infrastructure orchestration.
- **Project** - Provisioners often require multi-stage configurations which can be hosted as their own project.
- **Support Level** - The official install methods are categorized for production, development, and test usage. Other methods offer community support.
