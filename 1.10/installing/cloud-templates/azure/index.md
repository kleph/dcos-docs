---
post_title: Installing DC/OS using Azure Resource Manager templates
nav_title: Azure
menu_order: 20
---

This topic explains how to install DC/OS on Azure using the Azure Resource Manager templates.

**Warning:** Upgrades are not supported with this installation method. For a list of methods that support upgrading, see [Install DC/OS for Production](/docs/1.10/installing/).

# Install DC/OS

## Log into Azure

Visit <https://login.microsoftonline.com/> and enter your account credentials to log into Azure. You must have an active [Azure subscription](https://azure.microsoft.com/en-us/pricing/purchase-options/) to install DC/OS on Azure.

## Select number of masters

Visit the list of [Azure Resource Manager templates](https://downloads.dcos.io/dcos/stable/azure.html) and select the desired number of master nodes to deploy (1, 3, or 5).

- **One master node** is usually sufficient for demo purposes, but will not be highly available or recover from master node failure.
- **Three master nodes** provides high availability and recovery from master node failure.
- **Five master nodes** provides higher availability and can survive two simultaneous master node failures.

## Create an SSH key pair

An SSH key is required in order to log into DC/OS nodes or create an SSH tunnel.

- For Mac or Linux instructions, see [How to create and use an SSH public and private key pair for Linux VMs in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/mac-create-ssh-keys).
- For Windows instructions, see [How to Use SSH keys with Windows on Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/ssh-from-windows).

## Configure template

Most fields are filled in with defaults, but the following fields will need user specified values:

- **Resource group** - Specify the name of a new or existing resource group. Using a new group will make it easier to uninstall.
- **Master Endpoint DNS Name Prefix** - Specify a prefix to use in the master and public agent domain names.
- **Agent Endpoint DNS Name Prefix** - Specify a prefix to use in the private agent domain names.
- **SSH RSA Public Key** - Specify your public SSH key (named `id_rsa.pub` by default) to allow access to nodes in the cluster.

Other important fields:

- **Agent VM Size** - Specify the size of the agent VMs. Must have at least 2 CPU cores (default: `Standard_D2_v2`). See [Virtual Machines](https://azure.microsoft.com/en-us/pricing/details/virtual-machines/) for details on VM sizes.

## Purchase and deploy

Accept the Azure terms and conditions and select `Purchase` to create the Azure resources and install DC/OS. Then wait for the deployment to complete.

## Access DC/OS

For security reasons, DC/OS master nodes on Azure are inaccessible from the internet by default. You must use an SSH tunnel to access the DC/OS Dashboard or the DC/OS API. Because of this, the built-in DC/OS Oauth authentication is also disabled by default.

To use an SSH tunnel, find the `MASTERFQDN` (master fully qualified domain name) in the deployment output.

To find the deployment output, look under `Outputs` on the deployment page.

To find the deployment page, select `Resource groups` > [Your Resource Group Name] > `Deployments` > [Latest Deployment].

![Deployment history](/docs/1.10/img/dcos-azure-marketplace-step2a.png)

To copy, select the copy icon next to `MASTERFQDN` in the `Outputs` section.

![Deployment output](/docs/1.10/img/dcos-azure-marketplace-step2b.png)

Execute the following command in your local terminal, replacing `${MASTERFQDN}` with the value of `MASTERFQDN` (ex: `master-prefix.region.cloudapp.azure.com`) and `${KEY_PATH}` with the path to your private SSH key (ex: `~/.ssh/id_rsa`):

```bash
ssh azureuser@${MASTERFQDN} -p 2200 -L 8000:localhost:80 -i ${KEY_PATH} -fN -o ExitOnForwardFailure=yes
```

To access the DC/OS Dashboard, visit `http://localhost:8000/` on your local machine.

![DC/OS dashboard](/docs/1.10/img/dcos-gui.png)

To stop the SSH tunnel, execute the following terminal command:

```bash
ps aux | grep 'ssh azureuser' | grep -v grep | awk '{print $2}' | xargs kill
```

### Caveats

Some caveats around SSH access:

- The SSH tunnel will only work on the machine (virtual or physical) that it was executed from. If executed inside a Virtual Machine, only that virtual machine will have access. If executed from a physical machine, any virtual machine that shares the same host network may have access.
- In the example above, port `8000` is assumed to be available on your local machine. If not, select a different port to use for the SSH tunnel.
- The SSH commands above require a Linux or Mac terminal. To set up port forwarding on Windows with [Putty](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) see [SSH tunnel to endpoints in Azure VNet from Windows](https://blogs.msdn.microsoft.com/pliu/2017/01/17/ssh-tunnel-to-endpoints-in-azure-vnet-from-windows/).
- To learn more about SSH key generation check out this [GitHub tutorial](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/).
- The DC/OS UI will not show the correct IP address or CLI install commands when connected by using an SSH tunnel.

### Install the CLI on a master node

The following terminal commands can be used to install the DC/OS CLI directly onto a master node:

```bash
# Connect to a master node with ssh
ssh azureuser@${MASTERFQDN} -p 2200 -i ${KEY_PATH}

# Install CLI on the master node and configure with http://localhost
curl https://downloads.dcos.io/binaries/cli/linux/x86-64/dcos-1.10/dcos -o dcos
sudo mv dcos /usr/local/bin
sudo chmod +x /usr/local/bin/dcos
dcos cluster setup http://localhost
dcos
```

To stop the SSH session, type `exit` into the terminal.

## Next steps

- [Add users to your cluster](/docs/1.10/security/user-management/)
- [Install the DC/OS Command-Line Interface (CLI)](/docs/1.10/cli/install/)
- [Scaling considerations](https://azure.microsoft.com/en-us/documentation/articles/best-practices-auto-scaling/)
- [Uninstall DC/OS on Azure](/docs/1.10/installing/cloud-templates/azure/uninstalling/)
