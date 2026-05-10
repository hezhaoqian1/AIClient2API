# Railway Deployment

This fork is configured for Railway GitHub deployments with the root `Dockerfile`.

## Railway service setup

1. Create a Railway project.
2. Add a new service from GitHub.
3. Select `hezhaoqian1/AIClient2API`.
4. Railway should detect `railway.toml` and build with the Dockerfile.
5. Generate a public domain for the service.

## Required variables

Set a strong API key before exposing the service:

```env
ARGS=--api-key replace-with-a-strong-random-key
```

The container command already binds to `0.0.0.0` and uses Railway's `PORT`.
Only include `--port` in `ARGS` if you intentionally want to override Railway's runtime port.

## Persistent config

Add a Railway volume mounted at:

```text
/app/configs
```

Without the volume, UI changes, provider pool config, OAuth credentials, and the
admin password file can be lost on redeploy.

## Admin UI

After deployment, open the Railway public domain. The login page is available at:

```text
https://<your-railway-domain>/login.html
```

The upstream default admin password is stored in `configs/pwd`. Change it from
the UI after the first login.

## OAuth callback ports

The main API and UI run on Railway's HTTP port. Some OAuth flows may require
additional callback ports such as `8085`, `8086`, or `19876-19880`. Railway only
routes the public HTTP domain to the configured service port by default, so those
flows may need separate Railway TCP/public networking setup or credential import
from a local/VPS run.
