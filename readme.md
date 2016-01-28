#【U盘】

格式化为 FAT32，容量4G-8G。

mkfs.vfat -F 32 -n FIX /dev/sdX1

#【grub4dos】

* File: grub4dos-0.4.4-2009-12-03.zip
* MD5:  71bfd24294dcabac3e7a8326bb7ea88b
* 主页：https://github.com/chenall/grub4dos

1.解压，将 chinese/grldr 放到U盘根目录。Linux下，运行：

```bash
#use bootlace64.com instead of bootlace.com in x86_64 system
#install first stage boot loader to MBR of /dev/sdX
bootlace.com --no-backup-mbr --mbr-disable-floppy --time-out=0 /dev/sdX
```

另一种安装方法，使用dd。

```bash
dd if=./grldr.mbr of=/dev/sdX bs=440 count=1
dd if=./grldr.mbr of=/dev/sdX bs=1k skip=1 seek=1 count=8 
```

2.编写 menu.lst

【archboot】archboot/{initramfs_x86_64.img,intel-ucode.img,vmlinuz_x86_64}

【win7PE】WIN7PE3.1网络版，将ISO复制到 U盘/ISO/WIN7PE3.1.iso。

【小马2K3PE】: 已有 minipe/ntldr，需

* 复制minipe目录
* setup/myins/{deepin.xpm.gz,dos2pe.img,muifont.gz,mype} -> minipe/
* ezboot/{dostool.img,pm805.img} -> minipe/
* winpe.im\_ -> minipe/
* setup/myins/menu.lst -> minipe/xiaoma.lst
* setup/myins/{ntdetect.com,winnt.xpe} -> U盘根目录

【IMG】img目录, DOS98.IMG + MenuetOS.IMG。

【变色龙】chameleon + Extra/

【veket】镜像文件解压至veket目录即可。

【debian系 livecd】镜像iso复制到debian，配合 menu.lit 复制文件。


#【EFI 的 grub2 + clover】

1.GRUB2 

archbootISO/efi/grub/grubx64.efi -> U盘/efi/boot/bootx64.efi

添加 U盘/efi/boot/grubx64.cfg

2.tools

archbootISO/efi/tools/{shellx64_v1.efi,shellx64_v2.efi} ->
U盘/efi/tools/{shellx64_v1.efi,shellx64_v2.efi}

3.Clover

U盘/efi/Clover/

启动方式，不能直接通过grub2，须 grub2 -> shellx64_v2 -> clover.efi

4.编辑grubx64.cfg，添加locale, theme, font.

【archboot】

【debian系的livecd】

【Win8 x64 PE】我心如水_Win8_x64_PE_v19.36.ISO, only in UEFI

* sources目录
* boot目录
* EFI/microsoft目录
* EFI/BOOT/BOOTX64.EFI -> U盘/EFI/microsoft/boot/bootmgr.efi
