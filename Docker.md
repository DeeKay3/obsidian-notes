### Push image to Docker and Attach
Create Docker registry if needed:
```
docker run -d -p 32000:5000 --name registry registry:2
```
If already exists, check with:
```
docker ps -a
```
and start with:
```
docker start {container_id}
```
Push images to the registry (example is for indico):
```
sudo rockcraft.skopeo --insecure-policy copy   --dest-tls-verify=false   oci-archive:indico_1.0_amd64.rock   docker://localhost:32000/indico:latest
```
```
sudo rockcraft.skopeo --insecure-policy copy   --dest-tls-verify=false   oci-archive:indico-nginx_1.0_amd64.rock   docker://localhost:32000/indico-nginx:latest
```

Attach resources ( example is for indico ):
```
juju attach-resource indico indico-image=localhost:32000/indico:latest
juju attach-resource indico indico-nginx-image=localhost:32000/indico-nginx:latest
```

To list pushed images:
```
curl http://localhost:32000/v2/_catalog
```

Check specific image tags:
```
curl http://localhost:32000/v2/{myapp}/tags/list


```