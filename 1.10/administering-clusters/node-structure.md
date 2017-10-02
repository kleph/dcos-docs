---
post_title: Node Structure
menu_order: 5.0
---

DC/OS installation creates the following folders on each node:
<table class="table">
<tr>
<th>Folder</th>
<th>Description</th>
</tr>
<tr>
<td><code>/opt/mesosphere</code></td>
<td>Contains all the DC/OS binaries, libraries, cluster configuration. Do not modify.</td>
</tr>
<tr>
<td><code>/etc/systemd/system/dcos.target.wants</code></td>
<td>Contains the systemd services which start the things that make up systemd. They must live outside of `/opt/mesosphere` because of systemd constraints.</td>
</tr>
<tr>
<td><code>/etc/systemd/system/dcos.&lt;units&gt;</code></td>
<td>Contains copies of the units in `/etc/systemd/system/dcos.target.wants`. They must be at the top folder as well as inside `dcos.target.wants`.</td>
</tr>
<tr>
<td><code>/var/lib/dcos/exhibitor/zookeeper</code></td>
<td>Contains the [ZooKeeper](/docs/1.10/overview/concepts/#zookeeper) data.</td>
</tr>
<tr>
<td><code>/var/lib/docker</code></td>
<td>Contains the Docker data. </td>
</tr>
<tr>
<td><code>/var/lib/dcos</code></td>
<td>Contains the DC/OS and Mesos Master data.</td>
</tr>
<tr>
<td><code>/var/lib/mesos</code></td>
<td>Contains the Mesos Agent data.</td>
</tr>
</table>

**Important:** Changes to `/opt/mesosphere` are unsupported. They can lead to unpredictable behavior in DC/OS and prevent upgrades.
