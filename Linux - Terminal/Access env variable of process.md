Get process id from:
```
ps aux
```
If needed, get the user of the process, too. Run:
```
cat /proc/{process_id}/environ
```
If permission denied because process is used by other user, run:
``` 
sudo -u {user} bash -c 'cat /proc/{process_id}/environ'
```