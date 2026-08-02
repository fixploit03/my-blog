---
layout: post
title: "Crack PMKID Menggunakan Aircrack-NG"
date: 2026-08-02 03:20:00 +0700
tags: [wifi-hacking, aircrack-ng, pmkid]
---

![]({{ "/assets/img/crack-pmkid-menggunakan-aircrack-ng.png" | relative_url }})

## Persyaratan
- Aircrack-ng versi 1.4 ke atas
- File capture yang berisi paket EAPOL M1
- Keamanan Wi-Fi: WPA2-PSK

## Perintah

```bash
aircrack-ng -I <pmkid>*<mac_ap>*<mac_client>*<essid_hex> -w <wordlist>
```

> **Catatan**: MAC address AP dan client ditulis tanpa titik dua.

Contoh:

```bash
aircrack-ng -I 8d5dbb8be1eb630f6174415b8d10c729*5a5b585a55c7*9ee6fa71cd51*4b414e54494e2042454c414b414e47 -w rockyou.txt
```
