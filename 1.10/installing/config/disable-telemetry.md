---
post_title: Disable Telemetry
menu_order: 40
---

By default, DC/OS collects anonymous telemetry about user and system activity and sends it to Mesosphere.

To disable telemetry reporting, add this parameter to your install configuration ([`config.yaml`](/docs/1.10/installing/config/reference/)):

```yaml
telemetry_enabled: 'false'
```

Alternatively, a flag can be passed to the [CLI Installer](/docs/1.10/installing/development/cli-installer/) or [Advanced Installer](/docs/1.10/installing/production/advanced-installer/):

```
dcos_generate_config.sh --cli-telemetry-disabled
```

To disable telemetry reporting on an existing cluster, perform an upgrade to the same version of DC/OS using a new configuration.

Some install methods, like the simple AWS cloud templates, may not allow disabling authentication because they do not allow customization of [`config.yaml`](/docs/1.10/installing/config/reference/).
