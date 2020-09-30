# Troubleshoot disk IO

## Htop 

Add cpu details : F2 > Display options > Detailed CPU time
Add cpu average bar.

## Iostats :

Display extended statistic every second :
```bash
> iostat -x 1
avg-cpu:  %user   %nice %system %iowait  %steal   %idle
          20.74    0.00    4.93    0.63    0.00   73.69

Device            r/s     w/s     rkB/s     wkB/s   rrqm/s   wrqm/s  %rrqm  %wrqm r_await w_await aqu-sz rareq-sz wareq-sz  svctm  %util
loop0            0.00    0.00      0.00      0.00     0.00     0.00   0.00   0.00    0.00    0.00   0.00     0.00     0.00   0.00   0.00
nvme0n1       1369.00    0.00   6996.00      0.00     7.00     0.00   0.51   0.00    0.11    0.00   0.00     5.11     0.00   0.00   0.00
sda              0.00    0.00      0.00      0.00     0.00     0.00   0.00   0.00    0.00    0.00   0.00     0.00     0.00   0.00   0.00
dm-0          1376.00    0.00   6996.00      0.00     0.00     0.00   0.00   0.00    0.13    0.00   0.18     5.08     0.00   0.12  17.20
dm-1          1346.00    0.00   5384.00      0.00     0.00     0.00   0.00   0.00    0.12    0.00   0.17     4.00     0.00   0.12  16.80
dm-2            30.00    0.00   1612.00      0.00     0.00     0.00   0.00   0.00    0.27    0.00   0.01    53.73     0.00   0.13   0.40
dm-3             0.00    0.00      0.00      0.00     0.00     0.00   0.00   0.00    0.00    0.00   0.00     0.00     0.00   0.00   0.00
```

## Iotop :
Track all system disk usage :

```bash
sudo iotop
```
