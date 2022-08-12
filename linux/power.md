# Power management

Informations on cpu idle stat and wakeup calls : `powertop`

Better power management using : http://linrunner.de/en/tlp/docs/tlp-linux-advanced-power-management.html#configuration
Tlp doc site : https://linrunner.de/tlp/introduction.html

## Setup

```bash
apt install -t bullseye-backports tlp tlp-rdw
```

View battery information : `sudo tlp-stat -s`

Edit `/etc/tlp.conf` to activate custom threshold :

```
# BAT0: Primary / Main / Internal battery (values in %)
# Note: also use for batteries BATC, BATT and CMB0
# Default: <none>

START_CHARGE_THRESH_BAT0=75
STOP_CHARGE_THRESH_BAT0=80
```

View status : `sudo tlp-stat -s`

## Start a full charge :

```bash
$sudo tlp fullcharge
Setting temporary charge thresholds for BAT0:
  stop  = 100
  start =  96
Charging starts now, keep AC connected.
```