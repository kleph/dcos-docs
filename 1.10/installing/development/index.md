---
post_title: Install DC/OS for Development & Testing
nav_title: Development
menu_order: 10
---

The following methods may be used to deploy or install DC/OS for development and testing purposes.
They are generally bests suited for first time users, developers, continuous integration, and demos.
Because of this focus, these methods are generally less configurable, harder to upgrade, and not recommended for production use.

## Local Installer

DC/OS can be installed onto your workstation or laptop using one of the following methods:

- [dcos-vagrant](https://github.com/dcos/dcos-vagrant/) uses Vagrant and VirtualBox and works on Mac, Windows, and Linux
- [dcos-docker](https://github.com/dcos/dcos-docker/) uses Docker and works on Mac and Linux

## Cloud Templates

DC/OS can be installed in the cloud using one of the following methods:

- [AWS Cloud Formation Templates](/docs/1.10/installing/development/cloud-templates/aws/) comes in both basic and advanced varieties
- [Azure Resource Manager Templates](/docs/1.10/installing/development/cloud-templates/azure/resource-manager-templates/) uses Docker and works on Mac and Linux

## GUI Installer

Looking for a simple install wizard? Check out the

The [GUI Installer](/docs/1.10/installing/development/gui-installer/) is a simple browser-based install wizard. It can be used to install DC/OS onto any existing physical or virtual machines, cloud or on-premises.

## CLI Installer

The [CLI Installer](/docs/1.10/installing/development/cli-installer/) is an command-line installation tool that can be easily automated. It can be used to install DC/OS onto any existing physical or virtual machines, cloud or on-premises.
