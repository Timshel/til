# Firmware update using fwupd

Sources [github](https://github.com/fwupd/fwupd) and https://fwupd.org/

## Install

Debian package available:

```bash
sudo apt install fwupd
```

## Basic usage

If you have a device with firmware supported by fwupd, this is how you can check
for updates and apply them using fwupd's command line tools.

```bash
fwupdmgr get-devices
```

Download the latest metadata from LVFS.
```bash
fwupdmgr refresh
```

Display any updates available for any devices on the system.
```bash
fwupdmgr get-updates
```

Download and apply all updates for your system.

- Updates that can be applied live will be done immediately.
- Updates that run at bootup will be staged for the next reboot.

```bash
fwupdmgr update
```

You can find more information about the update workflow in the end
users section of the [fwupd website](https://fwupd.org).
