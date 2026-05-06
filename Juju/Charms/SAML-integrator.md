Create app from here:
https://manage.auth0.com/dashboard/us/dev-trwh15omio3lnvzp/
- Go to "Applications" -> "Create Application"
- Go to "Addons" and enable "SAML2"

### Deploy and configure 
Deploy:
```
juju deploy saml-integrator
```
Config:
```
juju config saml-integrator \
  entity_id="{issuer}" \
  metadata_url="{identity_provider_metadata_download_link}"
```
Integrate:
```
juju integrate {your_app}:saml saml-integrator:saml
```

### EDIT ZONE
```
juju config saml-integrator \
  entity_id="urn:dev-trwh15omio3lnvzp.us.auth0.com" \
  metadata_url="https://dev-trwh15omio3lnvzp.us.auth0.com/samlp/metadata/cjA3j1EuleObqXVvmk0gAJbTf1g6ZkwU"

juju integrate mattermost-k8s:saml saml-integrator:saml
```