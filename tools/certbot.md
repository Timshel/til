# Certificate handling

## Manual dns challenge

```bash
certbot run -a manual -d "*.toto.org"
```

And create the needed dns record : `_acme-challenge`.
To check that the record is correct :

```bash
dig @1.1.1.1 -t txt _acme-challenge.toto.org
```

Note: The generated challenge is stable (across the same day ?) so you can interrupt before the record is valided.

## Manual dns renew

Just rerun the command :

```bash
certbot run -a manual -d "*.toto.org"
```
