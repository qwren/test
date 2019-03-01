## system environment
```shell
$ apt-get install qemu libncurses5-dev gcc-arm-linux-gnueabi build-essential
```
## build rootfs 
```shell
$ git clone https://github.com/qwren/test.git
```
解压ramdisk.tar.bz2文件，并将得到的_install放到内核目录下面。
## build kernel
```shell
$ git clone https://github.com/torvalds/linux.git
$ cd linux
$ export ARCH=arm
$ export CROSS_COMPILE=arm-linux-gnueabi-
$ make vexpress_defconfig
$ make menuconfig
```
在General setup -> Initial RAM filesystem and RAM disk (initramfs/initrd) support上填入根文件系统名字，比如_install
```shell
$ make bzImage -j4 ARCH=arm CROSS_COMPILE=arm-linux-gnueabi-
$ make dtbs
```
## make sd image 
```shell
$ dd if=/dev/zero of=sd.img bs=1M count=32
$ mkfs.ext3 a9rootfs.ext3
```
## start qemu
```shell
$ qemu-system-arm -M vexpress-a9 -smp 4 -m 1024M -kernel arch/arm/boot/zImage -append "rdinit=/linuxrc console=ttyAMA0 loglevel=8"  -dtb arch/arm/boot/dts/vexpress-v2p-ca9.dtb -nographic -sd sd.img
```
进入qemu模拟环境，挂载sd卡
```shell
# mount -t ext3 dev/mmcblk0 tmp
```
