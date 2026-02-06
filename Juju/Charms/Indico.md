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
juju run s3-integrator/leader sync-s3-credentials access-key={access_key} secret-key={secret_key}
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

### ToDo:
- Refactor Indico terraform so that it has its own modules instead of the modules from the launchpad. Spec for this: https://docs.google.com/document/d/1EG71A2pJ244PQRaGVzGj7Mx2B_bzE4U_OSqx4eeVI1E/edit?tab=t.0

# Migration Steps:

1- Create env in PS7
	1- Request from IS the environment
	2- Migrate terraform to github from launchpad.
	3- Make sure that indico integration tests run in Canonical K8s successfully.
2- Upgrade and migrate from PS5 to PS7
	1- Use DB-as-a-service: https://canonical-information-systems-documentation.readthedocs-hosted.com/en/latest/products/managedPostgreSQL/how-to/request/
	**2- a) Migrate the data from PS5 to PS7.**  ( don't know how to )
	```
	current indico database 
	14.8 active 3 postgresql 14/edge 318
	```
	We need to figure out what Postgres version we get from IS DB service. In stg we have an deployed app with 14.19.
	3- Point the new environment to `events.canonical.com`
	4- Make sure backup is handled. Check if DB-service has it already or not. We need to test data migration before doing it in actual environment. ( dump and **restore**) 
	5- Get files in existing s3 buckets to new environment. Make sure the same files are working when upgrading from 3.3.1 to 3.3.8.
	6- Make sure to check new Redis is also working. 


PS5 Prod juju status output:
```
Model            Controller    Cloud/Region             Version  SLA          Timestamp
prod-events-k8s  prodstack-is  k8s-is-external/default  2.9.49   unsupported  13:43:02Z

SAAS                Status       Store         URL
grafana-dashboards  active       prodstack-is  admin/prod-cos-is-devops.grafana-dashboards
loki-logging        active       prodstack-is  admin/prod-cos-is-devops.loki-logging
pg                  active       prodstack-is  admin/prod-events-db.pg
prometheus-scrape   active       prodstack-is  admin/prod-cos-is-devops.prometheus-scrape
saml-integrator     maintenance  local         admin/prod-saml-integrator-k8s-ps5.saml-integrator

App                       Version  Status   Scale  Charm                     Channel        Rev  Address      Exposed  Message
indico                    3.3.1    active       2  indico                    latest/stable  213  10.85.1.111  no       
nginx-ingress-integrator  25.3.0   active       1  nginx-ingress-integrator  latest/stable   81  10.85.1.109  no       
redis-broker              7.0.4    active       1  redis-k8s                 latest/edge     24  10.85.0.193  no       
redis-cache               7.0.4    active       1  redis-k8s                 latest/edge     24  10.85.0.106  no       
s3-integrator                      waiting      1  s3-integrator             latest/edge     18  10.85.1.69   no       waiting for units to settle down
smtp-integrator                    active       1  smtp-integrator           latest/stable   10  10.85.0.145  no       

Unit                         Workload     Agent  Address       Ports  Message
indico/0                     active       idle   10.86.25.19          
indico/1*                    active       idle   10.86.64.229         
nginx-ingress-integrator/0*  active       idle   10.86.80.179         
redis-broker/0*              active       idle   10.86.73.207         
redis-cache/0*               active       idle   10.86.93.209         
s3-integrator/0*             maintenance  idle   10.86.73.202         
smtp-integrator/0*           active       idle   10.86.73.204 
```

- Indico plugin custom profile fields demo ([ISD-3338](https://warthogs.atlassian.net/browse/ISD-3338))
    
    - (Jay) Have legal name split into legal first and last name
    - (Jay) CAP details, I dont think we want people to fill these out. Could they be looked? Only editable by admins?
    - (Jay) Discussion about integrating Launchpad and Indico and HR to manage sprint groups
        
        - Not Indico and HRc directly
        - (Dogay) Will still look at HRc Indico integration to sync some fields, potentially
        
    - (Jay) Once the registration is created, can we add sections and move the fields?
        
        - (Dogay) In the plugin, I can have them put into a different section.
            
            - (Jay) We just want the CAP to be moved
            - Employee ID should not be editable
            - We could enable/disable the field as needed to make sure its populated but not visible
            
        
    - (Jay) The script pulls everyone as new users and it takes a few minutes for it to be remembered.
        
        - Because the fields use UUIDs, it would be hard to make the script work
        - (Jay) Other solution, with launchpad solution, we
        
    - (Dogay) What are the ops of the script
        
        - We use the script to bulk upload registration information
        - We prefill the data and upload it with the script
        - Indico does not do bulk registrations
        - So build registration first, then ask individuals to fill the rest of the fields
        - Script has function to check the mapping codes, generated field names.
        
    - (Jay) We would just need to setup a new template using the new fields. Not a problem.
    - The web team is helping us, not because it's their mandate, but because we ask very nicely.

git+https://github.com/canonical/canonical-indico-themes.git@854e1d814db1ccb350d8d7413dd4156154802d3f,git+https://github.com/canonical/indico-plugin-event-countdown.git@5303299107db010ccdf6fdc42ca8aa930dfa433b,https://github.com/canonical/flask-multipass-saml-groups/releases/download/1.2.3/flask_multipass_saml_groups-1.2.3-py3-none-any.whl,https://github.com/canonical/canonical-indico-personal-agenda/releases/download/v2.0.0/indico_plugin_personal_agenda-2.0.0-py3-none-any.whl,https://github.com/canonical/indico-custom-profile-fields/releases/download/1.1.0/indico_plugin_custom_profile_fields-1.1.0-py3-none-any.whl

juju deploy indico --channel=latest/edge --revision=295 --config external_plugins=git+https://github.com/canonical/canonical-indico-themes.git@854e1d814db1ccb350d8d7413dd4156154802d3f,git+https://github.com/canonical/indico-plugin-event-countdown.git@5303299107db010ccdf6fdc42ca8aa930dfa433b,https://github.com/canonical/flask-multipass-saml-groups/releases/download/1.2.3/flask_multipass_saml_groups-1.2.3-py3-none-any.whl,https://github.com/canonical/canonical-indico-personal-agenda/releases/download/v2.0.0/indico_plugin_personal_agenda-2.0.0-py3-none-any.whl,https://github.com/canonical/indico-custom-profile-fields/releases/download/1.1.0/indico_plugin_custom_profile_fields-1.1.0-py3-none-any.whl

export SERVICE_SCHEME="http"
export SERVICE_HOSTNAME="indico.local"
export INDICO_CONF="/etc/indico.conf"  # Ensure it points to the right config if needed
export CELERY_BROKER="redis://redis-broker-0.redis-broker-endpoints.indicotest.svc.cluster.local:6379"
export REDIS_CACHE_URL="redis://redis-cache-0.redis-cache-endpoints.indicotest.svc.cluster.local:6379"

indico shell

## Make Indico accept both normal logins and SAML/OIDC
In staging/prod: By default, Indico only accepts SAML logins when SAML is configured. To make it so that normal logins are also accepted in staging for testing ( do in all units ):
1- SSH into indico container:
```
juju ssh --container indico indico/0 bash
```
2- Install nano if not installed already:
```
apt update && apt install nano
```
3- Open indico.conf:
```
nano etc/indico.conf
```
4- Find `LOCAL_IDENTITIES` and change it to `True`
```
LOCAL_IDENTITIES = True
```
5- Reload uWSGI:
```
touch etc/uwsgi.ini
```
6- Restart pebble:
```
pebble restart indico
```