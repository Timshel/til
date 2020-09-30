# Curl

## Curl formatted output

The option `-w` allow to specify an output format :
Variables available : https://curl.haxx.se/docs/manpage.html

Ex:
```bash
>curl http://google.fr -o /dev/null -s -w '%{http_code} - %{time_total}\n'
301 - 0.020437
```

## Control curl dns resolution

Add the resolve parameter to control dns resolution :

```
curl -v https://domain.fr/ --resolve domain.fr:443:192.168.1.53
```