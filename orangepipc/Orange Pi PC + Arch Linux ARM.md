# Orange Pi PC + Arch Linux ARM: installation guide

This guide is based on multiple guides as well as official instructions for the other boards found on the Internet:

1. https://github.com/RoEdAl/alarm-uboot-sunxi-armv7
2. https://uthings.uniud.it/building-mainline-u-boot-and-linux-kernel-for-orange-pi-boards
3. https://archlinuxarm.org/platforms/armv7/allwinner/pcduino3

I have gone through all these steps recently and got a working board with my favorite Arch Linux ARM. I hope it will be helpful for someone else.  

## SD card

Replace `sdX` with the device name for your SD card.
Zero the beginning of the SD card:

```bash
sudo dd if=/dev/zero of=/dev/sdX bs=1M count=8
```

Run `fdisk` to partition the SD card:

```bash
sudo fdisk /dev/sdX
```

At the fdisk prompt, delete old partitions and create a new one:

1. Type `o`. This will clear out any partitions on the drive.
2. Type `p` to list partitions. There should be no partitions left.
3. Now type `n`, then `p` for primary, `1` for the first partition on the drive, `2048` for the first sector, and then press ENTER to accept the default last sector.
4. Write the partition table and exit by typing `w`.

Create the ext4 filesystem:

```bash
sudo mkfs.ext4 /dev/sdX1
```

Mount the filesystem:

```bash
sudo mount /dev/sdX1 /mnt
```

## Root filesystem

Download and extract the root filesystem:

```bash
wget http://os.archlinuxarm.org/os/ArchLinuxARM-armv7-latest.tar.gz
sudo bsdtar -xpf ArchLinuxARM-armv7-latest.tar.gz -C /mnt
sync
```

## Boot script

Install `uboot-tools`. In ArchLinux:

```bash
sudo pacman -S uboot-tools
```

Create `boot.txt` file in `/boot` dir.

```bash
cd /mnt/boot
nano boot.txt
```

Paste the following contents into it:

```
if test -n ${distro_bootpart}; then setenv bootpart ${distro_bootpart}; else setenv bootpart 1; fi
part uuid ${devtype} ${devnum}:${bootpart} uuid
setenv bootargs console=${console} root=PARTUUID=${uuid} rw rootwait

if load ${devtype} ${devnum}:${bootpart} ${kernel_addr_r} /boot/zImage; then
  if load ${devtype} ${devnum}:${bootpart} ${fdt_addr_r} /boot/dtbs/${fdtfile}; then
    if load ${devtype} ${devnum}:${bootpart} ${ramdisk_addr_r} /boot/initramfs-linux.img; then
      bootz ${kernel_addr_r} ${ramdisk_addr_r}:${filesize} ${fdt_addr_r};
    else
      bootz ${kernel_addr_r} - ${fdt_addr_r};
    fi;
  fi;
fi
```

Now create the boot script `boot.scr`:

```bash
sudo mkimage -A arm -O linux -T script -C none -n "U-Boot boot script" -d boot.txt boot.scr
```

Unmount your SD card:

```bash
sudo umount /mnt
cd /tmp
```

## U-Boot

Clone U-Boot repo and checkout the latest tag (or the one you need):

```bash

git clone --depth 1 --branch v2023.04  https://github.com/u-boot/u-boot
cd u-boot
```

Install the cross compiler for ARM EABI (in Arch Linux):

```bash
sudo pacman -S arm-none-eabi-gcc
```

Compile bootloader (you need to have `setuptools` Python package to be installed):

```bash
export CROSS_COMPILE=arm-none-eabi-
export ARCH=arm
make distclean
make orangepi_pc_defconfig
make
```

Flash it to the device:

```bash
sudo dd if=u-boot-sunxi-with-spl.bin of=/dev/sdX bs=1024 seek=8
```

## Booting

Insert the micro SD card into Orange Pi PC, connect Ethernet, and apply 5V power. Use the serial console or SSH to the IP address given to the board by your router. Login as the default user `alarm` with the password `alarm`. The default `root` password is `root`. Initialize the pacman keyring and populate the Arch Linux ARM package signing keys:

```bash
su
# Type in the password: root (by default)
pacman-key --init
pacman-key --populate archlinuxarm
```

And so on and so forth:

```bash
pacman -S sudo
visudo
```