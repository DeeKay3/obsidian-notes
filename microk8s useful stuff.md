Delete images ( sometimes need to refresh cache when uploading image ):
```
sudo microk8s ctr images list | awk '{print $1}' | xargs microk8s ctr images delete
```