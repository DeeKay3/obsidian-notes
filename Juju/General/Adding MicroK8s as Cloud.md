Install microk8s:
```
sudo snap install microk8s --classic
```
Join the group and set permissions:
```
sudo usermod -a -G microk8s $USER  
mkdir -p ~/.kube 
chmod 0700 ~/.kube
```
Close and re-open terminal or re-enter the session:
```
su - $USER
```
If needed, stop and restart service:
```
sudo snap stop microk8s
sudo snap start microk8s
```

Some add-ons to enable:
```
microk8s enable ingress
microk8s enable dns
microk8s enable hostpath-storage
```
Share the microK8s config with Juju:
```

sudo sh -c "mkdir -p /var/snap/juju/current/microk8s/credentials"
sudo sh -c "microk8s config | tee /var/snap/juju/current/microk8s/credentials/client.config"
```
Juju should detect microk8s and add it as cloud. Bootstrap controller:
```
juju bootstrap microk8s microk8s-controller
```
Arbitrary change.