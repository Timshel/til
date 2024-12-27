# Mastodon

## Setup :

Source : https://docs.joinmastodon.org/admin/install/

### Yarn

```bash
npm install -g yarn
```

### PG :

```bash
alter user mastodon with superuser;
alter user mastodon with nosuperuser;
```

Encoding issue :
https://stackoverflow.com/questions/16736891/pgerror-error-new-encoding-utf8-is-incompatible

```bash
UPDATE pg_database SET datistemplate = FALSE WHERE datname = 'template1';
DROP DATABASE template1;
CREATE DATABASE template1 WITH TEMPLATE = template0 ENCODING = 'UNICODE';
UPDATE pg_database SET datistemplate = TRUE WHERE datname = 'template1';
\c template1
VACUUM FREEZE;
```

### SMTP :

Cf : https://peterbabic.dev/blog/setting-up-smtp-in-mastodon/

In `.env.production` :

```conf
SMTP_SERVER=smtp.example.com
SMTP_PORT=465
SMTP_LOGIN=mastodon@peterbabic.dev
SMTP_PASSWORD=very-strong-passphrase-here
SMTP_FROM_ADDRESS=mastodon@peterbabic.dev
SMTP_SSL=true
SMTP_ENABLE_STARTTLS_AUTO=false
SMTP_AUTH_METHOD=plain
SMTP_OPENSSL_VERIFY_MODE=none
SMTP_DELIVERY_METHOD=smtp
```

## SSO using keycloak

Cf: https://frostillic.us/blog/posts/2022/11/10/tinkering-with-mastodon-keycloak-and-domino

```conf
OIDC_ENABLED=true
OIDC_DISPLAY_NAME=SSO auth
OIDC_DISCOVERY=true
OIDC_ISSUER=https://<keycloak_url>/realms/<real>
OIDC_AUTH_ENDPOINT=https://<keycloak_url>/realms/<real>/.well-known/openid-configuration
OIDC_SCOPE=openid,profile,email
OIDC_UID_FIELD=mastodonusername
OIDC_CLIENT_ID=<client id>
OIDC_CLIENT_SECRET=<client secret>
OIDC_REDIRECT_URI=https://<mastodon URL>/auth/auth/openid_connect/callback
OIDC_SECURITY_ASSUME_EMAIL_IS_VERIFIED=true
```

### Deploy :

#### nginx

Edit the conf file to update `server`, `server_name`, `root`  and `ssl_certificate`

#### Systemd

Edit the service files to add `Environment="BIND=0.0.0.0"` and fix the project path


### Admin :

Cf: https://docs.joinmastodon.org/admin/tootctl/

If post deploy you wiped the remote accounts assets :D :

```bash
RAILS_ENV=production /opt/rbenv/shims/bundle exec bin/tootctl accounts refresh --all
RAILS_ENV=production /opt/rbenv/shims/bundle exec bin/tootctl accounts refresh --domain mastodon.toto
```

Remove old media:

```bash
RAILS_ENV=production /opt/rbenv/shims/bundle exec bin/tootctl media remove --days=7
```
