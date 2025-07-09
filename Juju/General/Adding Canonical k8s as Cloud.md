First, remove microk8s and controllers for it (First remove controller)

Install and bootstrap:
```
sudo snap install k8s --channel=1.33-classic/stable --classic
sudo k8s bootstrap
```

To config with current k8s:
`sudo k8s config > ~/.kube/config`
`sudo chown $USER:$USER ~/.kube/config`

Add cloud to Juju:
`juju add-k8s ck8s --client`

Bootstrap controller:
`juju bootstrap ck8s ck8s-controller`

