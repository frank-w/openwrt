# Openwrt with Kernel 5.4 for BananaPi R2

## configuration
```sh
git clone https://www.github.com/frank-w/openwrt
cd openwrt
./scripts/feeds update -a
./scripts/feeds install -a
make menuconfig
```

change the following:

```
=> Target System (MediaTek Ralink ARM)
=> Subtarget (MT7623)
=> Target Profile (Bpi Banana Pi R2) 
=> Target Images
   => tar.gz
```
### kernel-configuration

make -j8 kernel_menuconfig

## build
```sh
make -j8 V=s
```

## install

using an sdcard with my debian-image and created a 3rd partition (ext4) using gparted and an up-to-date uboot using variables "root" and "console" from uEnv.txt

currently only rootfs to separate partition (mmcblk0p3)

```sh
sudo mkdir /mnt/openwrt
sudo mount /dev/sdx3 /mnt/openwrt
sudo tar -xzf ./bin/targets/mediatek/mt7623/openwrt-mediatek-mt7623-bpi_bananapi-r2-rootfs.tar.gz -C /mnt/openwrt
```

then change root-var in uEnv.txt to new partition and make sure, serial console is first in console-var, else you won't get a virtual terminal session

```sh
root=/dev/mmcblk0p3 rootfstype=ext4 rootwait
console=earlyprintk console=ttyS0,115200 console=tty1 fbcon=map:0
```
