## Deploy Indico
Deploy apps:
```auto
juju deploy postgresql-k8s --trust
juju deploy redis-k8s redis-broker --channel=latest/edge
juju deploy redis-k8s redis-cache --channel=latest/edge
juju deploy indico
```
Integrations:
```auto
juju integrate indico:redis-broker redis-broker
juju integrate indico:redis-cache redis-cache
juju integrate indico postgresql-k8s:database
```
or if Juju 2.9:
```auto
juju relate indico:redis-broker redis-broker
juju relate indico:redis-cache redis-cache
juju relate indico postgresql-k8s:database
```
Enable PSQL extensions:
```auto
juju config postgresql-k8s plugin_pg_trgm_enable=true plugin_unaccent_enable=true
```
Deploy and integrate nginx-ingress-integrator:
```auto
juju deploy nginx-ingress-integrator
juju trust nginx-ingress-integrator --scope cluster
juju integrate indico nginx-ingress-integrator
```
Add the following to `etc/hosts`:
```
127.0.0.1 indico.local
```
Deploy and integrate s3:
```shell
juju deploy s3-integrator
```
```bash
juju config s3-integrator \
    endpoint="https://s3.us-west-2.amazonaws.com" \
    bucket="testmydeploy8181" \
    path="/test" \
    region="us-west-2"
```
```bash
juju run s3-integrator/leader sync-s3-credentials access-key=AKIARI75FXOVBHHPDQN3 secret-key=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
```
```
juju integrate indico s3-integrator
```
# Push image to Docker and Attach
Create Docker registry if needed:
```
docker run -d -p 32000:5000 --name registry registry:2
```
Push the images to the registry:
```
sudo rockcraft.skopeo --insecure-policy copy   --dest-tls-verify=false   oci-archive:indico_1.0_amd64.rock   docker://localhost:32000/indico:latest
```
```
sudo rockcraft.skopeo --insecure-policy copy   --dest-tls-verify=false   oci-archive:indico-nginx_1.0_amd64.rock   docker://localhost:32000/indico-nginx:latest
```
Test password:
```
dplyLocaldevel2904
```

Attach resources:
```
juju attach-resource indico indico-image=localhost:32000/indico:latest
juju attach-resource indico indico-nginx-image=localhost:32000/indico-nginx:latest
```

# INDICO DEVELOPMENT APP INSTALLATION
Follow this URL:
```
https://docs.getindico.io/en/stable/installation/development/#install-dev
```

### For running after installation (included in guide):

Activate env:

```
source ./env/bin/activate
```

You will need two shells running in parallel. The first one will run the webpack watcher, which compiles the JavaScript and style assets every time you change them:

```
./bin/maintenance/build-assets.py indico --dev --watch
```

On the second one we’ll run the Indico Development server:
```
indico run -h <your-hostname> -q --enable-evalex
```
List plugins:
```
indico setup list-plugins
```

Fields to add (DELETE THIS):
```
The EA team has requested the addition of new fields on the user profile for Indico to avoid having to enter them on every event registration.

The fields are the following:

- Employee ID 
    
- Country
    
- T-shirt Size 
    
- Allergies 
    
- CAP details
    
- Group
    
- Product
    
- Pronouns (optional)
    
- Legal name
    

Ideally we would be syncing the following from the LDAP.

- Pronouns (optional)
    
- Legal name
    

We have to find out if Indico supports this out of the box and, if not, contribute the changes to add these fields upstream.
```
