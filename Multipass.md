Launch an instance with 4 cores, 16G RAM and 128G MemSpace ( you can name it with `-n`):
```
multipass launch -c 4 -m 16G -d 128G
```
Mount main folder:
```
multipass mount --type classic /home/dogay.kamar@canonical.com/Desktop/canonical-main {name-of-instance}

```
Shell into instance:
```
multipass shell {name-of-instance}
```