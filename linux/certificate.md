# Helper working with certificate


## Check a certificate details

```bash
> openssl x509 -in ~/cert2.pem -text -noout
...
> openssl x509 -enddate -noout -in cert.pem
notAfter=Aug 10 13:36:55 2029 GMT
```

## Check for validity of a signed certificate

```bash
> openssl verify -CAfile /etc/ssl/root.ca /etc/ssl/cert.pem
/etc/ssl/cert.pem: OK
```

## Check if the correct certificate is served :

`servername` is needed if the SNI is used on the server.

```bash
> openssl s_client -showcerts -connect ma.ttias.be:443  -servername iot-sail.dh.ssic-sail.fr
```