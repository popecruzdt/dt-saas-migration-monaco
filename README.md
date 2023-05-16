# dt-saas-migration-monaco

### Install Monaco
Follow Dynatrace's documentation to install the Monaco CLI here:\
https://www.dynatrace.com/support/help/shortlink/configuration-as-code-installation

### Update Manifest File
Modify the `manifest.yaml` file with the details for your Managed and SaaS environments.
1. Update your Managed environments, you only need to modify the environment url.value field
```
environmentGroups:
- name: managed
  environments:
  - name: managed_nonprod
    url:
      value: https://{your-domain}/e/{your-environment-id}
```
2. Update your SaaS environments, you only need to modify the environment url.value field
```
- name: saas
  environments:
  - name: saas_nonprod
    url:
      value: https://{your-environment-id}.live.dynatrace.com
```

### Generate Dynatrace API Tokens
Create new (or use existing) Dynatrace API Tokens with the following permissions (token scopes):
1. Managed
* Access problem and event feed, metrics, and topology
* Read synthetic monitors, locations, and nodes
* Read configuration
* Read settings
* Read SLO
* Read synthetic locations
2. SaaS
* Access problem and event feed, metrics, and topology
* Read synthetic monitors, locations, and nodes
* Read configuration
* Read settings
* Read SLO
* Read synthetic locations
* Write synthetic locations
* Write SLO
* Write settings
* Create and read synthetic monitors, locations, and nodes
* Write configuration