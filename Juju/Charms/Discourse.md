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


### Discourse upgrade PRs ToDos
- Check plugins for compatibility, if they are needed or bundled at core now etc.
- Check migrations and see if they need changes. Check the first migration applied in the new discourse version of core code and make necessary changes accordingly. Example PR:
https://github.com/canonical/discourse-k8s-operator/commit/dbfe37a9996beddcff0adcc6d05233f97b5b2b67

# TEMP - DELETE LATER
https://idp.internal.com/iam-hydra/.well-known/openid-configuration
3592c8dd-28eb-4e81-ab74-88576a9fb332
1qcN~VQNf9ydAs8GXc2hP_gYUu
openid email

Issues to solve
1) tlsv1 unrecognized name (SSL alert number 112)
	1) add to etc/hosts `10.64.140.43 idp.internal.com`. Try with external idp and see if this happens. If not, no need to fix via charm.
2) certificate verify failed 