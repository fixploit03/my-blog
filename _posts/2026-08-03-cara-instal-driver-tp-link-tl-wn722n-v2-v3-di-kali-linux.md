---
layout: post
title: "Cara Instal Driver TP-LINK TL-WN722N V2/V3 di Kali Linux"
date: 2026-08-03 01:37:00 +0700
tags: [wifi-hacking, tp-link, aircrack-ng]
---

## Instalasi

```bash
# update repositori kali linux
sudo apt update

# instal dependensi yang dibutuhkan
sudo apt install -y build-essential bc libelf-dev git dkms linux-headers-$(uname -r) realtek-rtl8188eus-dkms

# blacklist modul yang bermasalah
echo 'blacklist rtl8xxxu' | sudo tee -a /etc/modprobe.d/realtek.conf

# unload modul yang bermasalah
sudo rmmod rtl8xxxu

# load modul baru yang sudah diinstal
sudo modprobe 8188eu
```

## Verifikasi

```bash
lsmod | grep "8188eu"
```

## Testing

Aktifkan mode monitor:

```bash
sudo airmon-ng check kill
sudo airmon-ng start <interface>
```

Tes kemampuan packet injection:

```bash
sudo aireplay-ng --test <interface>
```

Scan Wi-Fi:

```bash
sudo airodump-ng <interface>
```
