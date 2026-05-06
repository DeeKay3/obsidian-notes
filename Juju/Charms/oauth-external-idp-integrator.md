Create oauth from here:
https://manage.auth0.com/dashboard/us/dev-trwh15omio3lnvzp/
- Go to "Applications" -> "Create Application"
- Go to "Settings", and add your Callback URL from your application to "Allowed Callback URLs"
- Deploy `oauth-external-idp-integrator` and config it like below, replacing Client ID, secret and domain from the settings of the auth0 app, and changing `scope` to whatever you need for your app.
  ```
juju config oauth-external-idp-integrator \
client_id="{CLIENT ID}" \
client_secret="{Client Secret}" \
issuer_url="https://{DOMAIN}" \
authorization_endpoint="https://{DOMAIN}/authorize" \
token_endpoint="https://{DOMAIN}/oauth/token" \
userinfo_endpoint="https://{DOMAIN}/userinfo" \
jwks_endpoint="https://{DOMAIN}/.well-known/jwks.json" \
scope="openid email profile" \
introspection_endpoint="https://{DOMAIN}/userinfo"
  ```
Integrate your charm:
```
juju integrate oauth-external-idp-integrator:oauth {app-name}
```
### EDIT AND COPY ZONE
```
juju config oauth-external-idp-integrator \
client_id="cjA3j1EuleObqXVvmk0gAJbTf1g6ZkwU" \
client_secret="JY0aTLMPyMHqpwgUvL6ZDCWWOp728k8fx5wMHJyO4oiIXeL3pDAXu7GWRjSHJdi4" \
issuer_url="https://dev-trwh15omio3lnvzp.us.auth0.com" \
authorization_endpoint="https://dev-trwh15omio3lnvzp.us.auth0.com/authorize" \
token_endpoint="https://dev-trwh15omio3lnvzp.us.auth0.com/oauth/token" \
userinfo_endpoint="https://dev-trwh15omio3lnvzp.us.auth0.com/userinfo" \
jwks_endpoint="https://dev-trwh15omio3lnvzp.us.auth0.com/.well-known/jwks.json" \
scope="openid email profile" \
introspection_endpoint="https://dev-trwh15omio3lnvzp.us.auth0.com/userinfo"
```
    application-data:
      audience: '[]'
      grant_types: '["authorization_code"]'
      redirect_uri: https://test.mattermost.com//callback
      scope: openid profile email
      token_endpoint_auth_method: client_secret_basic
    application-data:
      authorization_endpoint: https://dev-trwh15omio3lnvzp.us.auth0.com/authorize
      client_id: cjA3j1EuleObqXVvmk0gAJbTf1g6ZkwU
      client_secret_id: secret://8fb8ae91-09d7-4873-86d2-d7aac2084d9d/d7t6v5fmp25c761p5jmg
      introspection_endpoint: https://dev-trwh15omio3lnvzp.us.auth0.com/userinfo
      issuer_url: https://dev-trwh15omio3lnvzp.us.auth0.com
      jwks_endpoint: https://dev-trwh15omio3lnvzp.us.auth0.com/.well-known/jwks.json
      jwt_access_token: "False"
      scope: openid email profile
      token_endpoint: https://dev-trwh15omio3lnvzp.us.auth0.com/oauth/token
      userinfo_endpoint: https://dev-trwh15omio3lnvzp.us.auth0.com/userinfo
