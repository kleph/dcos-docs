---
post_title: Install Docker on CentOS
menu_order: 2
---
Docker's <a href="https://docs.docker.com/engine/installation/linux/centos/" target="_blank">CentOS-specific installation instructions</a> are always going to be the most up to date for the latest version of Docker. However, the following recommendations and instructions should make it easier to manage the Docker installation over time and mitigate several known issues with various other configurations.

# Recommendations

In addition to the general [Docker requirements and recommendations for DC/OS][1], the following CentOS-specific recommendations will improve your DC/OS experience.

- Use Docker's yum repository to install Docker.
  - The yum repository makes it easy to upgrade and automatically manages dependency installation.
- Use OverlayFS.
  - OverlayFS avoids known issues with `devicemapper` in `loop-lvm` mode and allows containers to use docker-in-docker, if they want.
- Format node storage volumes as XFS with the `ftype=1` option.

  ```bash
  mkfs -t xfs -n ftype=1 /dev/sdc1
  ```
  - As of CentOS 7.2, "<a href="https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/7.2_Release_Notes/technology-preview-file_systems.html" target="_blank">only XFS is currently supported for use as a lower layer file system</a>".
  - As of CentOS 7.3, the `ftype=1` option is used by default when formatting XFS volumes.
- Use CentOS 7.3.
  - As of CentOS 7.2, OverlayFS support has been improved with a fix for <a href="https://github.com/docker/docker/issues/10294" target="_blank">a bug with XFS</a>.
  - As of CentOS 7.3, OverlayFS is enabled by default.

# Instructions

The following instructions demonstrate how to use Docker with OverlayFS on CentOS 7.2+

1.  Verify that the kernel is at least 3.10:

    ```
    uname -r
    3.10.0-327.10.1.el7.x86_64
    ```

    If not, you may need a newer version of CentOS.

1.  Enable OverlayFS (CentOS 7.2 only):

    ```bash
    sudo tee /etc/modules-load.d/overlay.conf <<-'EOF'
    overlay
    EOF
    ```

    -   Reboot to reload kernel modules:

        ```bash
        reboot
        ```

    -   Verify that OverlayFS is enabled:

        ```bash
        lsmod | grep overlay
        overlay
        ```

1.  Configure yum to use the Docker yum repo:

    ```bash
    sudo tee /etc/yum.repos.d/docker.repo <<-'EOF'
    [dockerrepo]
    name=Docker Repository
    baseurl=https://yum.dockerproject.org/repo/main/centos/$releasever/
    enabled=1
    gpgcheck=1
    gpgkey=https://yum.dockerproject.org/gpg
    EOF
    ```

1.  Configure systemd to run the Docker Daemon with OverlayFS:

    ```bash
    sudo mkdir -p /etc/systemd/system/docker.service.d && sudo tee /etc/systemd/system/docker.service.d/override.conf <<- EOF
    [Service]
    ExecStart=
    ExecStart=/usr/bin/dockerd --storage-driver=overlay
    EOF
    ```

1.  Install the Docker engine, systemd service, and selinux plugin.

    ```bash
    sudo yum install -y docker-engine-1.13.1 docker-engine-selinux-1.13.1
    sudo systemctl start docker
    sudo systemctl enable docker
    ```

    When the process completes, you should see:

    ```
    Complete!
    Created symlink from /etc/systemd/system/multi-user.target.wants/docker.service to /usr/lib/systemd/system/docker.service.
    ```

1. Test that Docker is properly installed:

    ```bash
    sudo docker ps
    ```

For more generic Docker requirements, see [System Requirements: Docker][1].

[1]: /docs/1.10/installing/advanced-installer/system-requirements/#docker
