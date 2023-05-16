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

### Create Project Directories
Create the migration project directories if they don't already exist, inside this base directory (`dt-saas-migration-monaco`).
```
mkdir projects/migration/saas_nonprod
```
```
mkdir projects/migration/saas_prod
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

### Set Access Token Environment Variables
Dynatrace API Token values should not be stored in code, set them as environment variables each time you work with monaco.
```
export managed_nonprod_token=dt0c01.<your-api-token-for-managed-nonprod>
```
```
export managed_prod_token=dt0c01.<your-api-token-for-managed-prod>
```
```
export saas_nonprod_token=dt0c01.<your-api-token-for-saas-nonprod>
```
```
export saas_prod_token=dt0c01.<your-api-token-for-saas-prod>
```

### Download a setting configuration from Managed
Start by downloading a single settings schema from the `managed_nonprod` environment.\
Reference the `download` command documentation for guidance:\
https://www.dynatrace.com/support/help/shortlink/configuration-as-code-commands#download
```
monaco download --manifest manifest.yaml --environment managed_nonprod --force --output-folder projects/download --project managed_nonprod --settings-schema "builtin:management-zones" 
```
A project folder called `managed_nonprod` is created and contains a directory `builtinmanagement-zones`
```
ls -l projects/download/managed_nonprod/
```
> builtinmanagement-zones

You can download specific configurations by specifying `--settings-schema "<schema-id>"`.\
You can download all configurations by omitting the `--settings-schema` argument.  Note this will result in an overwhelming number of configurations in large environments.

### Copy setting configurations from Managed downloads to SaaS project directory
Copy the contents of `projects/download/managed_nonprod` that you want to migrate to SaaS to the `projects/migration` directory.  For example, copy `projects/download/managed_nonprod/builtinmanagement-zones` so that you end up with `projects/migration/saas_nonprod/builtinmanagement-zones`.
```
cp -r projects/download/managed_nonprod/ projects/migration/saas_nonprod/
```

### Deploy setting configurations project to SaaS environment
Apply the setting configurations from the `saas_nonprod` project to the `saas_nonprod` environment using the `deploy` command.\
Reference the `deploy` command documentation for guidance:\
https://www.dynatrace.com/support/help/shortlink/configuration-as-code-commands#deploy
##### Dry Run
```
monaco deploy tmp-manifest.yaml --environment saas_nonprod --project saas_nonprod --dry-run
```
##### Execute
```
monaco deploy tmp-manifest.yaml --environment saas_nonprod --project saas_nonprod
```