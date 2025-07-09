Access to bastion (PS6 for example):
```bash
ssh -v deekay@is-charms-bastion-ps6.internal
```

Load creds:

```
load_creds openstack
```

In a container in bastion, you'll have to use proxy. Find the proxy settings:
```
juju model_config
```
Then to use proxy inside a container ( for example using pip, in an `stg-indico` container in PS6 ):
```
HTTPS_PROXY=http://squid.ps6.internal:3128 pip install [...]
```
