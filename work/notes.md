# notes

## Linux Kernel build

build/.config

ls arch/loongarch/boot/dts/loongson

[*] Enable builtin dtb in kernel
    (loongson_2k2100) Built in DTB

DTB设备树中带有`earlycon`参数，`boot.cfg`中又`earlycon=uart,0x1fe001e0`了一次，需要把`boot.cfg`中`earlycon`参数删了。

```bash
console=ttyS0,115200 earlycon=uart,mmio,0x1fe001e0
```

## TSN

### PMON config

```bash
set run "ifconfig syn0 10.20.4.233;load http://10.20.4.61/vm;g console=ttyS0,115200 root=/dev/sda3"

set run "ifconfig syn0 10.20.4.234;load http://10.20.4.61/vm;g console=ttyS0,115200 root=/dev/sda3"

$run
```

### LinuxPTP

```bash
# 主时钟： 
ptp4l -i enp0s3f1 -m -H -2 -P
# 从时钟： 
ptp4l -i enp0s3f1 -m -H -s -2 -P
```
