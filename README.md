# Abel-Build

build system for project abel

# Boot Overview

Abel is based on Rochchip RK3328 platform. The reference design is ROCK64 SBC from Pine64.org.

Abel boots from emmc or mmc, which is partitioned in the way similar to Armbian image for ROCK64. It is different from that of Pine64/ayufan release for ROCK64 (rockchip official way).

In Armbian flavor, emmc is partitioned using MSDOS format. There is no raw partition for kernel (boot.img). 

Kernel, initramfs, and dtb files are placed in `/boot` folder on root file system. U-boot script is responsible for loading those files from ext4 file system and boot the kernel.


```
+--------+----------------+----------+---------------+--------------+
| part   | Terminology #1 | Actual   | Rockchip      | Image        |
| number |                | program  | Image         | Location     |
|        |                | name     | Name          | (sector)     |
+--------+----------------+----------+---------------+--------------+
|        | Secondary      | U-Boot   | idbloader.img | 0x0000000040 | miniloader
|        | Program        | SPL      | idbspl.img    |              |
|        | Loader (SPL)   |          |               |              |
|        |                |          |               |              |
|        |  -             | U-Boot   | u-boot.itb    | 0x0000004000 | u-boot
|        |                |          | u-boot.bin    |              |
|        |                |          | uboot.img     |              | 
|        |                |          |               |              |
|        |                | ATF      | trust.img     | 0x0000006000 | arm trusted firmware
|        |                |          |               |              |
| 1      |  P             |          | boot.img      | 0x0000008000 | ext4
|        |                |          |               |              |
| 2      |  A             | rootfs a | rootfs.img    | 0x0000040000 | ext4
|        |                |          |               |              |
| 3      |  B             | rootfs b | rootfs.img    | 0x0180040000 | ext4
+--------+----------------+----------+---------------+--------------+
```

## Partition P

Partition P is used to store boot scripts as well as persistent data.

`/boot` stores `boot.scr`, `boot.cmd`, `env.txt`.

`/data` stores persistent data surviving a firmware upgrade.

`/tmp` stores tmp data.

## Partition A/B

A/B model is used to upgrade the firmware. U-boot selects rootfs A or B according to `env.txt`.














