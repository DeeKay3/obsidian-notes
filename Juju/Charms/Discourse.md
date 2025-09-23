HOLY BIBLE: https://docs.google.com/document/d/1UoI6HMk0uhbe6I9LtyisRvnj9dJ5_OsOSqRdzjyRpvw/edit?tab=t.0#heading=h.6y09d6388wwe
juju Deploy:
```auto
juju deploy redis-k8s --channel latest/edge
juju deploy postgresql-k8s --channel 14/stable --trust
juju deploy discourse-k8s --channel edge
juju config postgresql-k8s plugin_hstore_enable=True plugin_pg_trgm_enable=True
juju integrate redis-k8s discourse-k8s
juju integrate discourse-k8s postgresql-k8s
juju deploy nginx-ingress-integrator
juju trust nginx-ingress-integrator --scope=cluster
juju integrate discourse-k8s nginx-ingress-integrator
```
Create admin:
```
juju run discourse-k8s/0 create-user admin=true email=email@example.com
```
Add this to `\etc\hosts`:
```
127.0.0.1 discourse-k8s
```

### Pass config option
I needed to pass `max_category_nesting` and for discourse, it is passed through environment variables, add it here:
https://github.com/canonical/discourse-k8s-operator/blob/033738328816672716acb1927bc745b8dd5a56e4/src/charm.py#L442

Configs have `DISCOURSE_` prefix, so for `max_category_nesting`, it would be `DISCOURSE_MAX_CATEGORY_NESTING` and then get the value from charm config.


