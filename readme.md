# 【U盘】

格式化为 FAT32，容量4G-8G。

mkfs.vfat -F 32 -n FIX /dev/sdX1

# 【grub4dos】

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

【archboot】`archboot/{initramfs_x86_64.img,intel-ucode.img,vmlinuz_x86_64}`

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

【debian系 livecd】镜像iso复制到debian, 配合 menu.lit 复制文件, debian ubuntu 不同 bootoptions

【grub2-filemanager】https://github.com/a1ive/grub2-filemanager


# 【EFI 的 grub2 + clover】

1.GRUB2 

archbootISO/efi/grub/grubx64.efi -> U盘/efi/boot/bootx64.efi

添加 U盘/efi/boot/grubx64.cfg

2.tools

```
archbootISO/efi/tools/{shellx64_v1.efi,shellx64_v2.efi} ->
U盘/efi/tools/{shellx64_v1.efi,shellx64_v2.efi}
```

3.Clover

U盘/efi/Clover/

启动方式，不能直接通过grub2，须 `grub2 -> shellx64_v2 -> clover.efi`

4.编辑grubx64.cfg，添加locale, theme, font.

【archboot】

【debian系的livecd】镜像iso复制到debian, 根据文件名修改 debian-iso-is-here

【grub2-filemanager】https://github.com/a1ive/grub2-filemanager

【Multiboot USB】[fork](https://github.com/hackerncoder/multibootusb), 复制 mbusb.d -> U盘/efi/boot/


# 【win7 8 10 11 等PE】

winpe 文件夹已放入 bootmgr 和 bootmgr.efi，并均已添加到 grub4dos 和 grub2。

* bootmgr 用于 BIOS(Legacy) 启动，读取 `boot/BCD`。
  BCD 中添加了 Win7(default), Win8, Win10, Windows Setup 四个启动项。
  确认 wim sdi 文件的路径可查看 `boot/bios-BCD.txt`。
* bootmgr.efi 用于 UEFI 启动，读取 `EFI/microsoft/boot/BCD`。
  BCD 中添加了 Win8(default), Win10, Win11, Windows Setup 四个启动项。
  确认 wim sdi 文件的路径可查看 `EFI/microsoft/boot/uefi-BCD.txt`。

* 不同 PE 的 `boot.sdi` (System Deployment Image) 虽然 md5 不同，
  但现阶段在 BCD 中都用 960k 的 `boot/boot.sdi`。
  若需要设置不同的 sdi 文件，可以用 `winpe/tools/BOOTICEx64_2016.06.17_v1.3.4.0.exe`
  在 PE 环境中修改 BCD。
* BIOS(Legacy) 与 UEFI 两种启动模式，启动项中的 path 是不同的
    - `\windows\system32\boot\winload.exe` vs `\windows\system32\boot\winload.efi`

用到的 PE 以及需要复制的 wim 文件。（文件名大小写不敏感）

* 【Win7 x86 PE】Only BIOS。WIN7PE3.1网络版。
    - `WIN7PE3.1.iso/boot/win7pe.wim` -> `winpe/Win7PEx86.wim`
* 【Win8 x64 PE】BIOS+UEFI。`我心如水_Win8_x64_PE_v19.36.ISO`
    - `我心如水_Win8_x64_PE_v19.36.ISO/sources/boot.wim` -> `winpe/Win8PEx64.wim`
* 【Win10 x64 PE】BIOS+UEFI。[微PE工具箱](https://www.wepe.com.cn)，`WePE_64_V2.3.exe` 导出 ISO。
    - `WePE_64_V2.3.iso/WEPE/WEPE64.WIM` -> `winpe/Win10WePEx64.wim`
* 【Win11 x64 PE】Only UEFI。[FirPE](https://firpe.cn)，`FirPE-V1.9.1.exe` 导出 ISO。
    - `FirPE-V1.9.1.iso/boot/11pex64.wim` -> `winpe/Win11FirPEx64.wim`
    - 11pex64.wim 内有两个卷，默认用卷 1，不影响 BCD 中的路径设置。
* 【Windows安装】BIOS+UEFI。`Windows 安装盘.iso`
    - windows安装盘/{sources,support} -> U盘/

winpe 文件夹空间占用情况：
```bash
[$] ls -lh FIX/winpe/
总计 1.5G
-rw-r--r-- 1  389K 2013年10月 9日 bootmgr
-rw-r--r-- 1  1.3M 2013年10月 9日 bootmgr.efi
drwxr-xr-x 3  4.0K  9月 5日 13:28 tools
-rw-r--r-- 1  210M  9月 5日 13:30 Win10WePEx64.wim
-rw-r--r-- 1  631M  9月 5日 13:31 Win11FirPEx64.wim
-rw-r--r-- 1  272M 2010年 7月29日 Win7PEx86.wim
-rw-r--r-- 1  410M 2014年 1月26日 Win8PEx64.wim
```
