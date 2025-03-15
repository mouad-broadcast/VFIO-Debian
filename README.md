# VFIO-Debian 12 (bookworm)
VFIO GPU Passthrough IOMMU 

## 1 ) Get the IDS
```
lspci -nnk | grep -i nvidia

```

## 2 ) Change GRUb config

```
vi /etc/default/grub
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on iommu=pt"
update-grub
reboot
# check after the reboot if iommu is enabled 
dmesg | grep -e DMAR -e IOMMU
```
## 3 ) Passthrough and isolation gpu from the host
```
vi /etc/modprobe.d/vfio.conf
options vfio-pci ids=10de:2489,10de:228b
```
```
vi /etc/modprobe.d/blacklist.conf
blacklist nouveau
blacklist nvidia
blacklist nvidiafb
```

## 4 ) Enable vfio modules
```
vi /etc/modules
vfio
vfio_pci
vfio_iommu_type1
vfio_virqfd
```
```
update-initramfs -u
reboot
```

## 5 )  check 

```
lspci -nnk  | grep --color -i -C 3 vfio 
```
## ++ qeum snapshot

#Create a snapshot:
```
qemu-img snapshot -c <snap-name> <file>.qcow2
```
#List snapshots:
```
qemu-img snapshot -l <file>.qcow2
```
#Revert to a snapshot:
```
qemu-img snapshot -a <snap-name> <file>.qcow2
```
#Delete a snapshot:
```
qemu-img snapshot -d <snap-name> <file>.qcow2
```
