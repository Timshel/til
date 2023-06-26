# LVM

Sources :
 - https://wiki.archlinux.org/index.php/LVM
 - https://linux.die.net/man/8/lvresize
 - https://gist.github.com/runozo/7313081630e164a70d67f81179523ad0

## Shrink the root partition 
The partition need to be unmounted otherwise there is important risk of corruption.
Using `resizefs` prevent errors when shrinking (vs using `resize2fs`).

```bash
> lvresize --resizefs -L -62G /dev/ubuntu-vg/root
```

## Increase the swap
For swap you can't use `resizefs` but it can be done live :

```bash
> sudo swapoff -v /dev/ubuntu-vg/swap_1
> sudo lvresize -L 62G /dev/ubuntu-vg/swap_1
> sudo mkswap /dev/VolGroup00/LogVol01
> sudo swapon -va
```

## Extend and resize FS

```bash
> lvextend  --resizefs -L +10G /dev/data/mastodon-public
```
