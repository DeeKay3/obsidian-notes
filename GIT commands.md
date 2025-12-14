### To rebase main into branch
```
git checkout main
git pull origin main
git checkout {branch-name}
git rebase main #Follow commands here
git push --force-with-lease origin {branch-name}
```
### To merge main into branch
```
git checkout main
git pull origin main
git checkout {branch-name}
git merge main #Follow commands here
git push origin {branch-name}
```