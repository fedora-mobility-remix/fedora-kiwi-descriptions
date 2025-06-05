# install fedora 43 phosh on oneplus 6

### disclaimer

- all data on you device will be wiped
- this is work-in-progress
- it may be not possible to update your system

### download fedora

```sh
curl -LO https://github.com/fedora-remix-mobility/fedora-kiwi-descriptions/releases/download/2025-04-04/Fedora-SDM845-Remix.tar.xz
tar -xf Fedora-SDM845-Remix.tar.xz
cd artifacts
```

### download uboot

- https://github.com/fedora-remix-mobility/u-boot/releases
- for oneplus 6 you need uboot-sdm845-oneplus-enchilada.img
- for oneplus 6t you need uboot-sdm845-oneplus-fajita.img

### flash uboot

```sh
fastboot erase dtbo_a
fastboot erase dtbo_b
fastboot flash boot uboot-sdm845-oneplus-*.img --slot=all
fastboot reboot
```

- after that uboot will be booted
- in uboot you should select "enable usb mass storage"
- all partitions on your phone will be mounted on your pc

### getting partition names

```sh
lsblk -o NAME,PARTLABEL | grep -E 'op2|system|userdata'
```

here is output i got on my device:

```sh
├─sda7  op2
├─sda13 system_a
├─sda14 system_b
└─sda17 userdata
```

### writing fedora on oneplus with dd

- esp.raw -> op2
- boot.raw -> system_a
- boot.raw -> system_b
- root.raw -> userdata

```sh
sudo dd if=esp.raw  of=/dev/sda7  bs=4M status=progress
sudo dd if=boot.raw of=/dev/sda13 bs=4M status=progress
sudo dd if=boot.raw of=/dev/sda14 bs=4M status=progress
sudo dd if=root.raw of=/dev/sda17 bs=4M status=progress
sync
```

then reboot phone and boot to system

