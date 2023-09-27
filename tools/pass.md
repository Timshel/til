# Pass

Cf: https://www.passwordstore.org/

## Multiline

```pass
>pass insert -m toto
Enter contents of dev/github/recovery and press Ctrl+D when finished:
```

## Otp plugin

CF: https://github.com/tadfisher/pass-otp

## Create otp from secret

```bash
>pass otp insert toto
Enter otpauth:// URI for toto:
>otpauth://totp/toto?secret=AAAAAAAAAAAAAAAA&issuer=toto
```
