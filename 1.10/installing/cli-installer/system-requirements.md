---
post_title: System Requirements for Installing DC/OS using the CLI Installer
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

Docker must also be configured to use the proxy on the agent nodes _after installation_, because the CLI installer installs Docker as part of `--install-prereqs`. For details, see [Docker HTTP/HTTPS proxy](https://docs.docker.com/engine/admin/systemd/#httphttps-proxy).

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

The DC/OS CLI installer should be executed from the bootstrap node.

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

### Firewalld

On CoreOS, firewalld is not installed by default.

On RHEL and CentOS, firewalld must be stopped and disabled.

There is a <a href="https://github.com/docker/docker/issues/16137" target="_blank">known issue</a> that Docker interacts poorly with firewalld.
For more information, see the <a href="https://github.com/docker/docker/blob/v1.6.2/docs/sources/installation/centos.md#firewalld" target="_blank">Docker CentOS firewalld</a> documentation.
This issue may be <a href="https://github.com/moby/moby/pull/25592" target="_blank">fixed in Docker 17.x</a>.

Stop and disable firewalld with the following command:

```bash
sudo systemctl stop firewalld && sudo systemctl disable firewalld
```

Alternatively, firewalld must be configured to support the [Networking requirements](#networking-requirements).

# Next steps

- [Install DC/OS using the CLI Installer](/docs/1.10/installing/cli-installer/).
