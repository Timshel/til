# Check disk status

General disk info (from `smartmontools`) :
```
>sudo smartctl -a /dev/sda | less
```

Check test time estimate :
```
>sudo smartctl -c /dev/sda
```

Run tests :
```
>sudo smartctl -t <short|long|conveyance> /dev/sdc
```


To check blocks :
```
sudo badblocks -b 4096  -v /dev/sdc
```