To deploy local packed charm after running `charmcraft pack`:
```
juju deploy {path_to_charm_file} --resource {image_name}={image_path}
```
If a charm doesn't have a workload, no need for `--resource` flag. Otherwise, `image_name` can be fetched from `charmcraft.yaml` or `metadata.yaml` under `resources` tab.

For `image_path`:
- If using the same image from charmcraft, go to the Resources tab of the charm in Charmhub, copy the digest, and then edit below:
```

```