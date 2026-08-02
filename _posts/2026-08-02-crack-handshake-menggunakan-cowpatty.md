---
layout: post
title: "Crack Handshake Menggunakan Cowpatty"
date: 2026-08-02 11:55:00 +0700
tags: [wifi-hacking, cowpatty, handshake]
---

## Persyaratan
- Cowpatty
- File capture yang berisi 4-way handshake
- Keamanan Wi-Fi: WPA/WPA2-PSK

## Instalasi

```bash
sudo apt update
sudo apt install -y cowpatty
```

## Perintah

```bash
cowpatty -r <file_capture> -f <wordlist> -s <ssid> -v
```
