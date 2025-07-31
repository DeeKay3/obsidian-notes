 Get the postgreSQL pod:
`kubectl get pod -n {namespace}`

Exec into postgreSQL:
`kubectl exec -it {podname} --container postgresql -n {namespace} -- bash`

Run psql ( get info from related charm config):
PGPASSWORD={pgpassword} psql -U {username} -d {databasename} -h localhost

### Get postgresql host address inside unit
For example, for Discourse staging environment, after sshing into the unit, I do:
```
ps -ef | grep -i postgres
```
And look for address after `--listen_addresses`. Then I connect:
```
psql --host=10.142.40.106 --username=operator --list (to list databases)
psql --host=10.142.40.106 --username=operator discourse (to connect to discourse database)
```

FXOT0K209uWncdsn