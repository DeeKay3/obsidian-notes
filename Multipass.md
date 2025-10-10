Launch an instance with 4 cores, 16G RAM and 128G MemSpace ( you can name it with `-n`):
```
multipass launch -c 4 -m 16G -d 128G -n m1
```
Mount main folder:
```
multipass mount --type classic /home/dogay.kamar@canonical.com/Desktop/canonical-main m1

```
Shell into instance:
```
multipass shell m1
```