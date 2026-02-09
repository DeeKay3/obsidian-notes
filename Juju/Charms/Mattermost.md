
# Create the Docker image

```
docker build -t localhost:32000/mattermost . 
```

# If no Docker registry is running yet

`docker run -d -p 32000:5000 --name registry registry:2

# Push to local repo

```
docker push localhost:32000/mattermost
```

# Deploy

```
juju deploy ./mattermost-k8s_ubuntu-20.04-amd64.charm --config mattermost_image_path=localhost:32000/mattermost
```

# Deploy other stuff

```bash
juju deploy postgresql-k8s
# If your k8s cluster has RBAC enabled, you'll then need to run this:
juju trust postgresql-k8s --scope=cluster
# Deploy and enable TLS certificates for PostgreSQL connections.
juju deploy tls-certificates-operator
# Using self-signed certificates for demonstration purposes only.
juju config tls-certificates-operator generate-self-signed-certificates="true" ca-common-name="Test CA"
juju integrate postgresql-k8s tls-certificates-operator
# And now relate postgresql-k8s to mattermost-k8s
juju integrate mattermost-k8s postgresql-k8s:db
```
# Expose
```bash
juju config mattermost-k8s juju-external-hostname=mattermost-k8s
microk8s enable ingress
juju expose mattermost-k8s
```
# Change image 
```bash
juju config mattermost-k8s mattermost_image_path=localhost:32000/mattermost:v2
```
