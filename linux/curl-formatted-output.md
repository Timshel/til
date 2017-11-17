# Curl formatted output

The option `-w` allow to specify an output format :
Variables available : https://curl.haxx.se/docs/manpage.html

Ex: 
```bash
>curl http://google.fr -o /dev/null -s -w '%{http_code} - %{time_total}\n'
301 - 0.020437
```
