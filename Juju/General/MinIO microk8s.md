First enable minIO:
```
microk8s enable minio
```
Get the access key:
```
microk8s kubectl get secret -n minio-operator microk8s-user-1 -o jsonpath='{.data.CONSOLE_ACCESS_KEY}' | base64 -d && echo
```
Get the secret key:
```
microk8s kubectl get secret -n minio-operator microk8s-user-1 -o jsonpath='{.data.CONSOLE_SECRET_KEY}' | base64 -d && echo
```
Get the endpoint IP & Port:
```
microk8s kubectl get service minio -n minio-operator -o jsonpath='http://{.spec.clusterIP}:{.spec.ports[0].port}' && echo
```
Save these values!

Expose minio locally with portforward:
```
microk8s kubectl port-forward -n minio-operator service/microk8s-console 9090:9090
```
Use access key and secret key to login. Go to **Buckets** and then **Create Bucket** to create bucket.

### Deploy S3 Integrator
```
juju deploy s3-integrator

juju config s3-integrator \
    endpoint="<ENDPOINT_URL>" \
    bucket="<BUCKET_NAME>" \
    path="/test" \
    region="local"

juju run s3-integrator/leader sync-s3-credentials \
    access-key="<ACCESS_KEY>" \
    secret-key="<SECRET_KEY>"

juju integrate <charm> s3-integrator
```