Add microk8s as cloud: [[Adding MicroK8s as Cloud]]
Then, deploy iam-bundle. First clone the integration repo:
```shell
git clone https://github.com/canonical/iam-bundle-integration.git && cd iam-bundle-integration
git checkout v0.6.0
```
Then, apply terraform plan to deploy the stack:
```shell
terraform -chdir=examples/tutorial init
terraform -chdir=examples/tutorial apply -auto-approve
```
Once you see `iam` model all green, create a personal admin account:
```shell
juju run kratos/0 create-admin-account email=<your-email> username=<username>
```