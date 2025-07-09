 Get the postgreSQL pod:
`kubectl get pod -n {namespace}`

Exec into postgreSQL:
`kubectl exec -it {podname} --container postgresql -n {namespace} -- bash`

Run psql ( get info from related charm config):
PGPASSWORD={pgpassword} psql -U {username} -d {databasename} -h localhost
