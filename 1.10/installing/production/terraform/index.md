---
post_title: Install DC/OS using Terraform
nav_title: Terraform
menu_order: 20
---

You can use Terraform scripts to install DC/OS on DigitalOcean and Packet.

One way to install DC/OS for production is to automate the [Advanced Installer](/docs/1.10/installing/production/advanced-installer/) with [Terraform](https://www.terraform.io/).

Here are a few Terraform projects to get you started:


| Environment | Nodes | Project | Support Level | Docs |
|--------------------|--------------|-------------------|---------|---------------|
| AWS/Azure | Virtual | [bernadinm/terraform-dcos](https://github.com/bernadinm/terraform-dcos) | Community | N/A |
| AWS | Virtual | [zutherb/terraform-dcos](https://github.com/zutherb/terraform-dcos) | Community | N/A |
| AWS | Virtual | [dcos-up](https://github.com/kensuio/dcos-up) | Community | N/A |
| DigitalOcean | Virtual | [digitalocean-dcos-terraform](https://github.com/jmarhee/digitalocean-dcos-terraform) | Dev/Test | [docs](/docs/1.10/installing/production/terraform/digitalocean/) |
| Packet | Physical | [packet-terraform](https://github.com/dcos/packet-terraform) | Dev/Test | [docs](/docs/1.10/installing/production/terraform/packet/) |


## Further reading

Looking to install into other environments? Checkout the [full list of install methods](/docs/1.10/installing/).
