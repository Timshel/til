# Promox

## Virtualization and PCI passtrough

Sources :

    - https://natebent.com/2021/10/gpu-passthrough-in-proxmox/
    - https://gist.github.com/qubidt/64f617e959725e934992b080e677656f

On iommu :

    - https://vfio.blogspot.com/2014/08/iommu-groups-inside-and-out.html
    - https://vfio.blogspot.com/2015/10/intel-processors-with-acs-support.html
    - https://lkml.org/lkml/2013/6/18/738

Check that CPU virtualization is activated in BIOS.

Change the boot grub arguments in `/etc/default/grub` :

```bash
GRUB_CMDLINE_LINUX_DEFAULT="quiet amd_iommu=on iommu=pt pcie_acs_override=downstream,multifunction"
``` 

Add required modules in `/etc/modules`

```bash
vfio
vfio_iommu_type1
vfio_pci
vfio_virqfd
```

IOMMU interrupt remapping

```bash
echo "options vfio_iommu_type1 allow_unsafe_interrupts=1" > /etc/modprobe.d/iommu_unsafe_interrupts.conf
echo "options kvm ignore_msrs=1" > /etc/modprobe.d/kvm.conf
```

Adding device vendorId to VFIO :

```bash
$lspci
$lspci -n -s 01:00
$echo "options vfio_pci ids=8086:10c9"> /etc/modprobe.d/vfio.conf
```

Update 

```bash
update-initramfs -u
update-grub
``` 

## VM 

Conf file path `/etc/pve/qemu-server/100.conf`

For passtrough VM `Sytem / Machine` need to be `q35`. 


## OpenWRT

Sources :

    - https://www.jwtechtips.top/how-to-install-openwrt-in-proxmox/


Find latest openwrt release for x86_64 and use the combined squashfs then after creating a VM with no OS and detaching the created Disk.


```bash
wget https://downloads.openwrt.org/releases/21.02.3/targets/x86/64/openwrt-21.02.3-x86-64-generic-squashfs-combined.img.gz
gunzip openwrt-21.02.3-x86-64-generic-squashfs-combined.img.gz
qm importdisk 100 openwrt-21.02.3-x86-64-generic-squashfs-combined.img local-vm
```

Then attach the volume to the VM.
After boot should be accessible at `198.168.1.1` with no password.
