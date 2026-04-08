pip install --upgrade -c /tmp/constraints-4bc0ea67410e6465.txt git+https://github.com/canonical/canonical-indico-themes.git@854e1d814db1ccb350d8d7413dd4156154802d3f git+https://github.com/canonical/indico-plugin-event-countdown.git@5303299107db010ccdf6fdc42ca8aa930dfa433b https://github.com/canonical/flask-multipass-saml-groups/releases/download/1.2.3/flask_multipass_saml_groups-1.2.3-py3-none-any.whl https://github.com/canonical/canonical-indico-personal-agenda/releases/download/v2.0.0/indico_plugin_personal_agenda-2.0.0-py3-none-any.whl https://github.com/canonical/indico-custom-profile-fields/releases/download/1.2.1/indico_plugin_custom_profile_fields-1.2.1-py3-none-any.whl
```

Relation provider                     Requirer                              Interface            Type     Message
indico:grafana-dashboard              grafana-dashboards:grafana-dashboard  grafana_dashboard    regular  
indico:indico-peers                   indico:indico-peers                   indico-instance      peer     
indico:metrics-endpoint               prometheus-scrape:metrics-endpoint    prometheus_scrape    regular  
loki-logging:logging                  indico:logging                        loki_push_api        regular  
nginx-ingress-integrator:nginx-route  indico:nginx-route                    nginx-route          regular  
pg:database                           indico:database                       postgresql_client    regular  
redis-broker:redis                    indico:redis-broker                   redis                regular  
redis-broker:redis-peers              redis-broker:redis-peers              redis-peers          peer     
redis-cache:redis                     indico:redis-cache                    redis                regular  
redis-cache:redis-peers               redis-cache:redis-peers               redis-peers          peer     
s3-integrator:s3-credentials          indico:s3                             s3                   regular  
s3-integrator:s3-integrator-peers     s3-integrator:s3-integrator-peers     s3-integrator-peers  peer     
saml-integrator:saml                  indico:saml                           saml                 regular  
smtp-integrator:smtp-legacy           indico:smtp-legacy                    smtp                 regular 

```

## FOR POST MORTEM

- Make sure correct version is released in stable for the Indico charm
- Make sure you test the upgrade in the Juju version of prod. Prod was 2.9.49, we only tested in Juju 3 in local and staging