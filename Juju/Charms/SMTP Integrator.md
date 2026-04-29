For testing with an actual mail server, set up Mailpit locally:
### Run Mailpit on host
```
# Download and run the binary (listens on SMTP :1025, web UI :8025)

curl -sLo mailpit https://github.com/axllent/mailpit/releases/latest/download/mailpit-linux-amd64

chmod +x mailpit

./mailpit
```