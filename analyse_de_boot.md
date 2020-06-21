# Maitrise de poste - Sujet 4 - Analyse de Boot

> On réalisera ce TP sur une machine physique sous ArchLinux 5.6.15

**Exo 1**

**Déterminer les 5 processus les plus longs à démarrer au boot de l'OS.**

> La commande `systemd-analyze plot > graphe.svg` nous donne n graphe représentant les différents processus qui se sont lancés au démarrage de l'OS.
> On détermine donc assez facilement les 5 processus prenant le plus de temps au démarrage.
> Pour moi, il s'agit de :
> - man-db.service (34.227s)
> - dhcpcd@enp0s31f6.service (15.024s)
> - updatedb.service (14.355s)
> - lvm2-monitor.service (5.856s)
> - dev-sdc2.device (5.559s)


**Exo 2**

**Analyser les logs de boot du noyau** (avec la commande `dmesg`)

 - Déterminer la liste de périphériques vus par le noyau, et l'ordre dans lequel ils sont remontés.

> Ceci peut se faire assez facilement avec la commande `dmesg | grep device`, on obtient alors la sortie suivante : 
```
[    0.037132] [mem 0x90000000-0xdfffffff] available for PCI devices
[    0.079059] Console: colour dummy device 80x25
[    0.451799] pci 0000:01:00.0: vgaarb: setting as boot VGA device
[    0.451799] pci 0000:01:00.0: vgaarb: VGA device added: decodes=io+mem,owns=io+mem,locks=none
[    0.451799] usbcore: registered new device driver usb
[    0.487090] system 00:00: Plug and Play ACPI device, IDs PNP0c02 (active)
[    0.487570] pnp 00:01: Plug and Play ACPI device, IDs PNP0400 (active)
[    0.487599] pnp 00:02: Plug and Play ACPI device, IDs PNP0303 PNP030b (active)
[    0.487637] pnp 00:03: Plug and Play ACPI device, IDs PNP0f03 PNP0f13 (active)
[    0.487930] pnp 00:04: Plug and Play ACPI device, IDs PNP0501 (active)
[    0.488067] system 00:05: Plug and Play ACPI device, IDs PNP0c02 (active)
[    0.488144] system 00:06: Plug and Play ACPI device, IDs PNP0c02 (active)
[    0.488158] pnp 00:07: Plug and Play ACPI device, IDs PNP0b00 (active)
[    0.488188] system 00:08: Plug and Play ACPI device, IDs INT3f0d PNP0c02 (active)
[    0.488407] system 00:09: Plug and Play ACPI device, IDs PNP0c02 (active)
[    0.488443] system 00:0a: Plug and Play ACPI device, IDs PNP0c02 (active)
[    0.488711] system 00:0b: Plug and Play ACPI device, IDs PNP0c02 (active)
[    0.489816] system 00:0c: Plug and Play ACPI device, IDs PNP0c02 (active)
[    0.490629] pnp: PnP ACPI: found 13 devices
[    0.497336] pci 0000:01:00.0: Video device with shadowed ROM at [mem 0x000c0000-0x000dffff]
[    0.910265] fb0: EFI VGA frame buffer device
[    0.910669] input: Sleep Button as /devices/LNXSYSTM:00/LNXSYBUS:00/PNP0C0E:00/input/input0
[    0.910704] input: Power Button as /devices/LNXSYSTM:00/LNXSYBUS:00/PNP0C0C:00/input/input1
[    0.910734] input: Power Button as /devices/LNXSYSTM:00/LNXPWRBN:00/input/input2
[    0.997034] acpi device:79: hash matches
[    0.997040] acpi device:4c: hash matches
[    1.978627] usb usb1: New USB device found, idVendor=1d6b, idProduct=0002, bcdDevice= 5.06
[    1.978628] usb usb1: New USB device strings: Mfr=3, Product=2, SerialNumber=1
[    1.979865] usb usb2: New USB device found, idVendor=1d6b, idProduct=0003, bcdDevice= 5.06
[    1.979865] usb usb2: New USB device strings: Mfr=3, Product=2, SerialNumber=1
[    2.309426] usb 1-2: new full-speed USB device number 2 using xhci_hcd
[    2.317899] device-mapper: uevent: version 1.0.3
[    2.318097] device-mapper: ioctl: 4.42.0-ioctl (2020-02-27) initialised: dm-devel@redhat.com
[    2.451105] usb 1-2: New USB device found, idVendor=1b1c, idProduct=0c13, bcdDevice= 1.00
[    2.451110] usb 1-2: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[    2.572751] usb 1-4: new full-speed USB device number 3 using xhci_hcd
[    2.900868] usb 1-4: New USB device found, idVendor=0951, idProduct=16a4, bcdDevice= 0.05
[    2.900874] usb 1-4: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[    3.022737] usb 1-8: new full-speed USB device number 4 using xhci_hcd
[    3.164159] usb 1-8: New USB device found, idVendor=046d, idProduct=c33a, bcdDevice=14.00
[    3.164164] usb 1-8: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[    3.286069] usb 1-11: new full-speed USB device number 5 using xhci_hcd
[    3.427511] usb 1-11: New USB device found, idVendor=0d8c, idProduct=0135, bcdDevice= 1.00
[    3.427517] usb 1-11: New USB device strings: Mfr=1, Product=2, SerialNumber=0
[    3.549407] usb 1-12: new full-speed USB device number 6 using xhci_hcd
[    3.690766] usb 1-12: New USB device found, idVendor=046d, idProduct=c332, bcdDevice= 3.02
[    3.690771] usb 1-12: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[    4.770556] systemd[1]: Starting Create list of static device nodes for the current kernel...
[    4.909438] systemd[1]: Finished Create list of static device nodes for the current kernel.
[    7.246720] Console: switching to colour frame buffer device 128x48
[    7.455246] input: PC Speaker as /devices/platform/pcspkr/input/input5
[    8.100205] mei_me 0000:00:16.0: enabling device (0000 -> 0002)
[    8.615192] iTCO_wdt: Found a Intel PCH TCO device (Version=4, TCOBASE=0x0400)
[    8.654828] input: HDA NVidia HDMI/DP,pcm=3 as /devices/pci0000:00/0000:00:01.0/0000:01:00.1/sound/card1/input7
[    8.654854] input: HDA NVidia HDMI/DP,pcm=7 as /devices/pci0000:00/0000:00:01.0/0000:01:00.1/sound/card1/input8
[    8.654877] input: HDA NVidia HDMI/DP,pcm=8 as /devices/pci0000:00/0000:00:01.0/0000:01:00.1/sound/card1/input9
[    8.654899] input: HDA NVidia HDMI/DP,pcm=9 as /devices/pci0000:00/0000:00:01.0/0000:01:00.1/sound/card1/input10
[    8.654922] input: HDA NVidia HDMI/DP,pcm=10 as /devices/pci0000:00/0000:00:01.0/0000:01:00.1/sound/card1/input11
[    8.654944] input: HDA NVidia HDMI/DP,pcm=11 as /devices/pci0000:00/0000:00:01.0/0000:01:00.1/sound/card1/input12
[    8.654966] input: HDA NVidia HDMI/DP,pcm=12 as /devices/pci0000:00/0000:00:01.0/0000:01:00.1/sound/card1/input13
[    8.752140] input: HDA Intel PCH Front Mic as /devices/pci0000:00/0000:00:1f.3/sound/card0/input14
[    8.752211] input: HDA Intel PCH Rear Mic as /devices/pci0000:00/0000:00:1f.3/sound/card0/input15
[    8.752268] input: HDA Intel PCH Line as /devices/pci0000:00/0000:00:1f.3/sound/card0/input16
[    8.752323] input: HDA Intel PCH Line Out Front as /devices/pci0000:00/0000:00:1f.3/sound/card0/input17
[    8.752374] input: HDA Intel PCH Line Out Surround as /devices/pci0000:00/0000:00:1f.3/sound/card0/input18
[    8.752396] input: HDA Intel PCH Line Out CLFE as /devices/pci0000:00/0000:00:1f.3/sound/card0/input19
[    8.752472] input: HDA Intel PCH Line Out Side as /devices/pci0000:00/0000:00:1f.3/sound/card0/input20
[    8.752526] input: HDA Intel PCH Front Headphone as /devices/pci0000:00/0000:00:1f.3/sound/card0/input21
[    8.779460] Console: switching to colour dummy device 80x25
[    9.351680] input: Kingston HyperX 7.1 Audio Consumer Control as /devices/pci0000:00/0000:00:14.0/usb1/1-4/1-4:1.3/0003:0951:16A4.0001/input/input22
[    9.406163] input: Kingston HyperX 7.1 Audio as /devices/pci0000:00/0000:00:14.0/usb1/1-4/1-4:1.3/0003:0951:16A4.0001/input/input23
[    9.406573] input: Logitech G413 Carbon Mechanical Gaming Keyboard as /devices/pci0000:00/0000:00:14.0/usb1/1-8/1-8:1.0/0003:046D:C33A.0002/input/input24
[    9.463114] input: Logitech G413 Carbon Mechanical Gaming Keyboard as /devices/pci0000:00/0000:00:14.0/usb1/1-8/1-8:1.1/0003:046D:C33A.0003/input/input25
[    9.519724] input: Logitech G413 Carbon Mechanical Gaming Keyboard Consumer Control as /devices/pci0000:00/0000:00:14.0/usb1/1-8/1-8:1.1/0003:046D:C33A.0003/input/input26
[    9.520883] input: Logitech Gaming Mouse G502 as /devices/pci0000:00/0000:00:14.0/usb1/1-12/1-12:1.0/0003:046D:C332.0005/input/input30
[    9.521900] input: Logitech Gaming Mouse G502 Keyboard as /devices/pci0000:00/0000:00:14.0/usb1/1-12/1-12:1.1/0003:046D:C332.0006/input/input31
[    9.576343] input: Logitech Gaming Mouse G502 Consumer Control as /devices/pci0000:00/0000:00:14.0/usb1/1-12/1-12:1.1/0003:046D:C332.0006/input/input32
[    9.576552] input: Logitech Gaming Mouse G502 System Control as /devices/pci0000:00/0000:00:14.0/usb1/1-12/1-12:1.1/0003:046D:C332.0006/input/input33
[    9.596591] memmap_init_zone_device initialised 1572864 pages in 10ms
[    9.603212] nouveau 0000:01:00.0: DRM: DMEM: registered 6144MB of device memory
[    9.616277] mousedev: PS/2 mouse device common for all mice
[    9.879438] fbcon: nouveaudrmfb (fb0) is primary device
[   10.034289] Console: switching to colour frame buffer device 240x67
[   10.152659] nouveau 0000:01:00.0: fb0: nouveaudrmfb frame buffer device
```

 - Repérer la version du noyau utilisé
 
> Ceci peut être fait avec la commande `dmesg | grep linux`, et cela donne la sortie suivante : 
> ```
> [    0.000000] Linux version 5.6.15-arch1-1 (linux@archlinux) (gcc version > 10.1.0 (GCC)) #1 SMP PREEMPT Wed, 27 May 2020 23:42:26 +0000
> ```
> On remarque donc que j'utilise la version 5.6.15 du noyau Arch1-1
> 

 - Choisir et expliquer 5 autres lignes qui vous semblent importantes

Ici il s'agit des paramètres avec lesquelles le kernel démarre, il est possible de les modifier ponctuellement au boot, ou bien de manière permanente dans un fichier de config.
```
Command line: root=UUID=7b8b5b44-2f58-44c1-8768-ff15ec09691f rw quiet loglevel=3 systemd.show_status=false udev.log-priority=3 vt.global_cursor_default=0 splash initrd=boot\initramfs-linux.img
```

Ici il s'agit de la marque et et des caractéristiques de ma carte mère ainsi que de la version de son BIOS
```
DMI: Micro-Star International Co., Ltd. MS-7B61/Z370 GAMING PLUS (MS-7B61), BIOS 1.10 10/27/2017
```

Ici systemd démarre, il indique la version de l'OS et dans quel mode celui-ci est lancé, s'en suit ensuite les options utilisées.
```
systemd[1]: systemd 245.6-6-arch running in system mode.
```

Ici, systemd démarre `journalctl` qui est utile lorsque un processus ne tourne pas normalement (par ex. si le networkManager n'arrive pas a se connecter, les logs erreur, seront enregistrés par journalctl).
```
systemd[1]: Starting Journal Service...
```

Ici, systemd monte le système de fichiers nécessaire à ma configuration du Kernel, sans quoi les paramètres ne seraient pas enregistrés.
```
systemd[1]: Mounting Kernel Configuration File System...
```

 - Est-ce que le kernel continue à générer des logs une fois la machine allumée ?

> La commande `watch dmesg` nous permet de répondre simplement à cette question, a l'execution de cette commande l'output de `dmesg` est actualisé toutes les deux secondes, on remarque donc que lorsque un périphérique est connecté ou déconnecté, `dmesg` produit une sortie, le kernel produit donc encore des logs.
