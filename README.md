# dt-saas-migration-monaco

### Install Monaco
Follow Dynatrace's documentation to install the Monaco CLI here:\
https://www.dynatrace.com/support/help/shortlink/configuration-as-code-installation\

### Update Manifest File
Modify the `manifest.yaml` file with the details for your Managed and SaaS environments.
1. Update your Managed environments
```
environmentGroups:
- name: managed
  environments:
  - name: managed_nonprod
    url:
      value: https://{your-domain}/e/{your-environment-id}
```