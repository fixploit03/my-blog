---
layout: post
title: "Cara Instal Hcxdumptool (Build dari Source)"
date: 2026-08-04 15:45:00 +0700
tags: [wifi-hacking, hcxdumptool, pmkid]
---

## Instalasi

```bash
sudo apt update
sudo apt install -y build-essential git pkg-config libpcap-dev libssl-dev
git clone https://github.com/ZerBea/hcxdumptool.git
cd hcxdumptool
make
sudo make install
```

Verifikasi:

```bash
hcxdumptool --version
```
