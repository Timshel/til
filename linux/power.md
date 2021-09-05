# Power management

Informations on cpu idle stat and wakeup calls : `powertop`

Better power management using : http://linrunner.de/en/tlp/docs/tlp-linux-advanced-power-management.html#configuration

## Start a full charge :

```bash
$sudo tlp fullcharge
Setting temporary charge thresholds for BAT0:
  stop  = 100
  start =  96
Charging starts now, keep AC connected.
```