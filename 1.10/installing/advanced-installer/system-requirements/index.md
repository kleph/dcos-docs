---
post_title: System Requirements for Installing DC/OS using the Advanced Installer
nav_title: System Requirements
menu_order: 0
---

# Node types

DC/OS requires four node configurations:

- [Booststrap node](#bootstrap-node).
- [Master nodes](#master-node).
- [Public agent nodes](#public-agent-node).
- [Private agent nodes](#private-agent-node).

# Networking requirements

- The bootstrap node must be able to access the rest of the nodes over the network using SSH.
- The master and agent nodes must be able to access all other master and agent nodes over the network.
- The master nodes should be accessible from outside the cluster for administrative access.
- The private agent nodes should not be accessible from outside the cluster for security reasons.
- The public agent nodes should be accessible from outside the cluster for public ingress.
- The agent nodes must be able to access a Docker registry (ex: [Docker Hub](https://hub.docker.com/)) in order to run Docker containers.
- If you specify `exhibitor_storage_backend: zookeeper` in the [DC/OS configuration](/docs/1.10/installing/config/), the bootstrap node must remain after installation and be accessible from the master nodes. With `exhibitor_storage_backend: zookeeper`, the Mesos masters use the bootstrap ZooKeeper for leader election.
- If you specify `master_discovery: master_http_loadbalancer` in the [DC/OS configuration](/docs/1.10/installing/config/), the following TCP ports on the master nodes must be load balanced: 80, 443, 8080, 8181, 2181, 5050.

## Proxy configuration

If DC/OS can only reach the internet thought a proxy, `use_proxy`, `http_proxy`, `https_proxy`, and `no_proxy` must be specified in the [DC/OS configuration](/docs/1.10/installing/config/).

Docker must also be configured to use the proxy on the agent nodes. For details, see [Docker HTTP/HTTPS proxy](https://docs.docker.com/engine/admin/systemd/#httphttps-proxy).

## Port and protocol configuration

- The master and agent nodes must be accessible by IPv4 from all other nodes.
- The master and agent nodes must allow SSH ingress from the bootstrap node.
- The master and agent nodes must have Internet Control Message Protocol (ICMP) enabled.
- The master nodes must be accessible over UDP on port 53 from the agent nodes. The Mesos agent (`dcos-mesos-slave`) uses this port to discover the leading Mesos master (`leader.mesos`).

## High Speed Internet Access

High speed internet access is recommended for DC/OS installation. A minimum 10 MBit per second is required for DC/OS services.
The installation of some DC/OS services will fail if the artifact download time exceeds the value of `MESOS_EXECUTOR_REGISTRATION_TIMEOUT` within the `/opt/mesosphere/etc/mesos-slave-common` file.
The default value for `MESOS_EXECUTOR_REGISTRATION_TIMEOUT` is 10 minutes.

# Operating system

All nodes in the cluster should have the same operating system. The following operating systems are recommended:

*   **CentOS** - 7.3 recommended.
*   **RHEL** - 7.3 recommended.
*   **CoreOS** - 1235.12.0 recommended.

# Bootstrap node

The DC/OS Advanced installer should be executed from the bootstrap node.

## Hardware requirements

You must have only one bootstrap node with the following configuration:

|             | Minimum   | Recommended |
|-------------|-----------|-------------|
| RHEL/CentOS | 7.2, 7.3  | 7.3         |
| CoreOS      | 1235.9.0  | 1235.12.0   |
| Nodes       | 1         | 1           |
| Processor   | 2 cores   | 2 cores     |
| Memory      | 16 GB RAM | 16 GB RAM   |
| Hard disk   | 60 GB     | 60 GB       |

## Software requirements

- An **unencrypted SSH private key** must be available for authentication with the other nodes. Encrypted SSH keys are not supported.
- **Docker** must be installed (1.11.x - 1.13.x is recommended).
- The system locale must be configured. For example, set the `LC_ALL` and `LANG` environment variables to `en_US.utf-8`. (TODO: verify locale requirement)

### NGINX

The advanced installer produces install artifacts on the bootstrap node that must be served to the master and agent nodes.
To do so, install NGINX. For convenience, pre-cache the NGINX Docker image:

```bash
sudo docker pull nginx
```

# Master nodes

## Hardware requirements

You must have an odd number of master nodes with the following configuration:

|             | Minimum   | Recommended |
|-------------|-----------|-------------|
| RHEL/CentOS | 7.2, 7.3  | 7.3         |
| CoreOS      | 1235.9.0  | 1235.12.0   |
| Nodes       | 1         | 3 or 5      |
| Processor   | 4 cores   | 4 cores     |
| Memory      | 32 GB RAM | 32 GB RAM   |
| Hard disk   | 120 GB    | 120 GB      |

### Storage requirements

- The `/opt/mesosphere` directory cannot be on an LVM logical volume or shared storage. This directory is used for DC/OS component systemd services, which must be available at boot time.
- The `/tmp` directory must not be mounted with `noexec`. This directory is used to execute binaries during component bootstrapping (ex: Exhibitor and ZooKeeper).
- The `/var/lib/mesos` directory must not be remotely mounted. This directory is used for Mesos master and agent state.

Many component run on the masters. Some of them are I/O-intensive (ex: ZooKeeper log replication) while others are CPU-intensive (ex: network state synchronization).
We recommend the following:

- Solid-state drive (SSD)
- RAID controllers with a Backup Battery Unit (BBU)
- RAID controller cache configured in writeback mode


## Software requirements

### SSH server

SSH server must be installed and enabled for installer access.

CoreOS comes with the SSH server daemon (`sshd`) installed and enabled by default.

On RHEL and Centos, use the following command to install SSH Server:

```
yum install openssh-server
```

### NTP

Network Time Protocol (NTP) must be installed and enabled for clock synchronization.

CoreOS comes with NTP daemon (`ntpd`) installed and enabled by default.

On RHEL and Centos, use the following command to install NTP:

```
yum install ntp
```

To verify that NTP is installed and running, try one of the following commands (TODO: more info about applicability of NTP tests):

TODO: what OSes are these tools available?

- `ntptime`
- `adjtimex -p`
- `timedatectl`

### Data compression utilities

The <a href="http://www.info-zip.org/UnZip.html" target="_blank">UnZip</a>, <a href="https://www.gnu.org/software/tar/" target="_blank">GNU tar</a>, and <a href="http://tukaani.org/xz/" target="_blank">XZ Utils</a> data compression utilities must be installed on your master and agent nodes.

CoreOS comes with these utilities installed by default.

On RHEL and Centos, use the following command to install these utilities:

```bash
sudo yum install -y tar xz unzip curl ipset
```

### Linux user groups

The `nogroup` group must be present on the master and agent nodes.

Create this group, if it doesn't exist:

```bash
sudo groupadd nogroup
```

# Public agent nodes

The public and private agent node requirements are identical except for [Network requirements](#network-requirements).
For everything else, see [Private agent nodes](#private-agent-nodes).

# Private agent nodes

## Hardware requirements

You may have any number of agent nodes with the following configuration:

|             | Minimum   | Recommended |
|-------------|-----------|-------------|
| RHEL/CentOS | 7.2, 7.3  | 7.3         |
| CoreOS      | 1235.9.0  | 1235.12.0   |
| Nodes       | 1         | 6 or more   |
| Processor   | 2 cores   | 2 cores     |
| Memory      | 16 GB RAM | 16 GB RAM   |
| Hard disk   | 60 GB     | 60 GB       |

### Storage requirements

- The `/opt/mesosphere` directory cannot be on an LVM logical volume or shared storage. This directory is used for DC/OS component systemd services, which must be available at boot time.
- The `/tmp` directory must not be mounted with `noexec`. This directory is used to execute binaries during component bootstrapping (ex: Exhibitor and ZooKeeper).
- The `/var` directory must have at least 10 GB of free space. This directory is used for task sandboxes by both [Docker and DC/OS Universal container runtime](/docs/1.10/deploying-services/containerizers/).
- The `/var/lib/docker` directory must not be remotely mounted. This directory is used for Docker volumes.
- The `/var/lib/mesos` directory must not be remotely mounted. This directory is used for Mesos master and agent state.

## Software requirements

### SSH server

SSH server must be installed and enabled for installer access.

CoreOS comes with the SSH server daemon (`sshd`) installed and enabled by default.

On RHEL and Centos, use the following command to install SSH Server:

```
yum install openssh-server
```

### NTP

Network Time Protocol (NTP) must be installed and enabled for clock synchronization.

CoreOS comes with NTP daemon (`ntpd`) installed and enabled by default.

On RHEL and Centos, use the following command to install NTP:

```
yum install ntp
```

To verify that NTP is installed and running, try one of the following commands (TODO: more info about applicability of NTP tests):

- `ntptime`
- `adjtimex -p`
- `timedatectl`

### Data compression utilities

The <a href="http://www.info-zip.org/UnZip.html" target="_blank">UnZip</a>, <a href="https://www.gnu.org/software/tar/" target="_blank">GNU tar</a>, and <a href="http://tukaani.org/xz/" target="_blank">XZ Utils</a> data compression utilities must be installed on your master and agent nodes.

CoreOS comes with these utilities installed by default.

On RHEL and Centos, use the following command to install these utilities:

```bash
sudo yum install -y tar xz unzip curl ipset
```

### SELinux

SELinux must be configured to work with Docker.
For this reason Docker 1.13.1 is recommended, because it fixes a regression from 1.13.0 and 1.13.0 fixed a number of earlier issues.

If you can't use Docker 1.13.1, it is recommended to set SELinux to permissive mode:

```bash
sudo sed -i s/SELINUX=enforcing/SELINUX=permissive/g /etc/selinux/config
sudo setenforce 0
```

It may be required to restart the nodes after disabling SELinux in order for Docker engine to come up healthy, depending on the versions.

### Linux user groups

The `nogroup` and `docker` groups must be present on the agent nodes.

Create these groups, if they do not exist:

```bash
sudo groupadd nogroup
sudo groupadd docker
```

### Firewalld

On CoreOS, firewalld is not installed by default.

On RHEL and CentOS, firewalld must be stopped and disabled.

There is a <a href="https://github.com/docker/docker/issues/16137" target="_blank">known issue</a> that Docker interacts poorly with firewalld.
For more information, see the <a href="https://github.com/docker/docker/blob/v1.6.2/docs/sources/installation/centos.md#firewalld" target="_blank">Docker CentOS firewalld</a> documentation.
This issue may be <a href="https://github.com/moby/moby/pull/25592" target="_blank">fixed in Docker 17.x</a>.

Stop and disable firewalld with the following command:

```bash
sudo systemctl stop firewalld
sudo systemctl disable firewalld
```

Alternatively, firewalld must be configured to support the [Networking requirements](#networking-requirements).

### Docker

Docker must be installed on the bootstrap and agent nodes.

The following version of Docker are verified to work with DC/OS:

- 1.13.x
- 1.12.x
- 1.11.x

**Warning**: Docker 1.10.x and earlier have a <a href="https://github.com/docker/docker/issues/9718" target="_blank">serious bug related to concurrent pulls</a>.

Each operating system has its own Docker install instructions:

- **CoreOS** - Docker comes installed and configured by default. No further configuration should be required.
- **CentOS** - Follow the guide to [Install Docker on CentOS][2].
- **RHEL** - Either follow [Install Docker on CentOS][2] or install Docker via the RHEL subscription yum repository. For more information, see <a href="https://access.redhat.com/articles/881893" target="_blank">Docker Formatted Container Images on Red Hat Systems</a>.

For reference, see the official <a href="http://docs.docker.com/engine/installation/" target="_blank">Docker 1.13 install instructions</a>.

#### Docker supervision

The Docker engine needs to be restarted when it crashes. If necessary, create a systemd service to supervise the Docker engine.

#### Docker linux user

By default the Docker client must be run with the root user (or `sudo`). Alternatively, create a <a href="https://docs.docker.com/engine/installation/linux/centos/#create-a-docker-group" target="_blank">docker user group</a> to give the Docker client root privileges.

#### Docker storage drivers

Recommended storage drivers:

- `OverlayFS`.
- `devicemapper` in `direct-lvm`.

For more information, see Docker's <a href="https://docs.docker.com/engine/userguide/storagedriver/selectadriver/" target="_blank">Select a Storage Driver</a>.

**Warning**: Do not use `devicemapper` in `loop-lvm` mode. For more information, see [Docker and the Device Mapper storage driver](https://docs.docker.com/engine/userguide/storagedriver/device-mapper-driver/).

# Next steps

- [Install DC/OS using the Advanced Installer](/docs/1.10/installing/advanced-installer/).
