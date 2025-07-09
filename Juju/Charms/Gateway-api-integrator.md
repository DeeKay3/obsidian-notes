https://github.com/canonical/dex-auth-operator/pull/245/files
https://github.com/canonical/dns-operators/blob/main/dns-integrator-operator/src/charm.py
https://github.com/canonical/gateway-api-integrator-operator

any-charm for mocking provider/app

# To Deploy
Add model:
`juju add-model dns-record-requirer`

Deploy gateway-api-integrator charm:
```bash
juju deploy ./gateway-api-integrator\_amd64.charm --base ubuntu@24.04 --trust 
juju config gateway-api-integrator gateway-class=cilium 
juju config gateway-api-integrator external-hostname=www.example.com
juju deploy self-signed-certificates 
juju integrate gateway-api-integrator self-signed-certificates
```

For testing:
- Deploy app:
	```
	juju deploy flask-k8s --channel latest/edge
	juju integrate flask-k8s gateway-api-integrator:gateway 
	```
- Deploy bind ( any-charm ):
	```
	juju deploy any-charm --channel latest/beta
	juju integrate any-charm gateway-api-integrator:dns-record
	```

- Curl flask-k8s from ip ( to make sure it works )
- Make sure dns-record is added
- Curl flask-k8s through domain-name 

- Check relation data:
```
jhack show-relation httprequest-lego-provider bind
```
- Ask someone:
```
unit-any-charm-0: 11:54:12 INFO juju.worker.uniter.operation ran "install" hook (via hook dispatching script: dispatch)
unit-any-charm-0: 11:54:13 INFO juju.worker.uniter.operation ran "peer-any-relation-created" hook (via hook dispatching script: dispatch)
unit-any-charm-0: 11:54:13 INFO juju.worker.uniter found queued "leader-elected" hook
unit-any-charm-0: 11:54:14 INFO juju.worker.uniter.operation ran "leader-elected" hook (via hook dispatching script: dispatch)
unit-any-charm-0: 11:54:15 INFO juju.worker.uniter.operation ran "config-changed" hook (via hook dispatching script: dispatch)
unit-any-charm-0: 11:54:15 INFO juju.worker.uniter found queued "start" hook
unit-any-charm-0: 11:54:15 INFO unit.any-charm/0.juju-log Running legacy hooks/start.
unit-any-charm-0: 11:54:16 INFO juju.worker.uniter.operation ran "start" hook (via hook dispatching script: dispatch)
unit-any-charm-0: 11:54:50 INFO juju.worker.uniter.operation ran "provide-dns-record-relation-created" hook (via hook dispatching script: dispatch)
unit-any-charm-0: 11:54:51 INFO juju.worker.uniter.operation ran "provide-dns-record-relation-joined" hook (via hook dispatching script: dispatch)
unit-any-charm-0: 11:54:52 INFO juju.worker.uniter.operation ran "provide-dns-record-relation-changed" hook (via hook dispatching script: dispatch)
unit-any-charm-0: 11:54:53 INFO juju.worker.uniter.operation ran "provide-dns-record-relation-changed" hook (via hook dispatching script: dispatch)
unit-any-charm-0: 11:59:14 INFO juju.worker.uniter.operation ran "update-status" hook (via hook dispatching script: dispatch)

```