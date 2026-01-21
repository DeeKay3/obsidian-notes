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
juju model-config
```
Then to use proxy inside a container ( for example using pip, in an `stg-indico` container in PS6 ):
```
HTTPS_PROXY=http://squid.ps6.internal:3128 pip install [...]
```

## FOR INDICO/DISCOURSE
When running "terraform init --upgrade", you need to set proxy. Do this:
```
https_proxy=http://squid.internal:3128 NO_PROXY=radosgw.ps6.canonical.com  terraform init -upgrade -reconfigure
```

## SETTING UP PROXY RULES- LINKS
https://code.launchpad.net/canonical-is-internal-proxy-configs
https://canonical-information-systems-documentation.readthedocs-hosted.com/en/latest/explanation/prodstack/proxy/
https://canonical-information-systems-documentation.readthedocs-hosted.com/en/latest/how-to/prodstack/proxy/