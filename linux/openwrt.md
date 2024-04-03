# OpenWRT

## Password

Change it in `System / Administration`

## ONT

We can easily directl connect to the ONT
Need to pass a vendor ID to the ONT, prefix should be `NB6VAC` anything pass might be useless but works with the modele number found in the `/state` box page.

Create a new interface :

    - `General / Protocol` : `DHCP`
    - `Advanced Settings / Vendor Class` : `neufbox_NB6VAC-...`
    - `Firewall / Assign` : `wan`

And you should be ok, ONT will assign ip address.

## Upgrade

Cf : https://openwrt.org/docs/guide-user/installation/generic.sysupgrade#command-line_instructions

```ash
wget https://downloads.openwrt.org/releases/22.03.5/targets/x86/64/openwrt-22.03.5-x86-64-generic-squashfs-combined.img.gz
wget https://downloads.openwrt.org/releases/22.03.5/targets/ipq40xx/generic/openwrt-22.03.5-ipq40xx-generic-glinet_gl-b1300-squashfs-sysupgrade.bin
sha256sum
sysupgrade -v openwrt-...
```

System will reboot.
You will need to reinstall software (ex: `wireguard-tools` + `luci-app-wireguard`)
