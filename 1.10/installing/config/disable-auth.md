---
post_title: Disable Authentication
nav_title: Disable Auth
menu_order: 30
---

By default, DC/OS provides authentication using third-party OAuth providers (Google, Github, and Microsoft).

To disable authentication, add this parameter to your install configuration ([`config.yaml`](/docs/1.10/installing/config/reference/):

```yaml
oauth_enabled: 'false'
```

To disable authentication on an existing cluster, perform an upgrade to the same version of DC/OS using a new configuration.

Some install methods, like the simple AWS cloud templates, may not allow disabling authentication because they do not allow customization of [`config.yaml`](/docs/1.10/installing/config/reference/).
