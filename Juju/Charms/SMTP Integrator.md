For testing with an actual mail server, set up Mailpit locally:
### Run Mailpit on host
```
# Download and run the binary (listens on SMTP :1025, web UI :8025)
sudo sh < <(curl -sL https://raw.githubusercontent.com/axllent/mailpit/develop/install.sh)

mailpit
```
### Find host IP reachable from microk8s pods
```
ip route get 8.8.8.8 | awk '{print $7; exit}'
# e.g. 10.0.2.15
```
### Configure smtp-integrator
```
# Deploy (skip if already done)
juju deploy smtp-integrator --channel latest/stable
  
# Point it to Mailpit — no auth required
juju config smtp-integrator \
host=<HOST_IP> \
port=1025 \
domain=test.local \
transport_security=none \
auth_type=none
```