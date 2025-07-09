To check what possible IPs a URL may use ( or any other verbose ):
```
curl https://www.example.com -v
```
To resolve a website to use a specific IP ( staging Indico for example ):
```
curl -k --resolve events.staging.canonical.com:443:91.189.95.14 https://events.staging.canonical.com/ -v
 ```