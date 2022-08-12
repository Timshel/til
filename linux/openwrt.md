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

## 