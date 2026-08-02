---
layout: post
title: "Kenapa PMKID Bisa Dimanfaatkan untuk Mengcrack Passphrase WPA2-PSK?"
date: 2026-08-02 19:25:00 +0700
tags: [wifi-hacking, pmkid]
---

### Tahap 1 - Mendapatkan Data-Data yang Dibutuhkan

Untuk mengcrack PMKID attacker harus mengumpulkan data-data berikut ini:
- **PMKID**: Nilai hash yang berhasil di-capture dari EAPOL frame (M1)
- **SSID**: Nama Wi-Fi
- **MAC AP**: MAC address Access Point
- **MAC STA**: MAC address client
- **Wordlist**: File yang berisi kandidat password

> Data-data tersebut bisa didapatkan oleh attacker dengan cara melakukan capture trafik Wi-Fi menggunakan tools seperti `wireshark` atau `airodump-ng`.

### Tahap 2 - Menghitung PMK

PMK dihitung menggunakan rumus berikut:

```
PMK = PBKDF2(HMAC-SHA1, passphrase, SSID, 4096, 256)
```

Script python:

```py
#!/usr/bin/env python3
import hashlib

passphrase = "PASSPHRASE"  # password wi-fi
ssid = "SSID"              # nama wi-fi
pmk = hashlib.pbkdf2_hmac('sha1', passphrase.encode(), ssid.encode(), 4096, 32)

print("PMK:", pmk.hex())
```

### Tahap 3 - Menghitung PMKID

Setelah PMK didapat, PMKID dihitung menggunakan rumus:

```
PMKID = HMAC-SHA1(PMK, "PMK Name" || MAC_AP || MAC_STA)
```

Script python:

```py
#!/usr/bin/env python3
import hmac, hashlib, binascii

pmk = binascii.unhexlify("PMK")         # hasil pmk dari tahap 2
mac_ap = binascii.unhexlify("MAC_AP")   # tanpa titik dua
mac_sta = binascii.unhexlify("MAC_STA") # tanpa titik dua

data = b"PMK Name" + mac_ap + mac_sta
pmkid = hmac.new(pmk, data, hashlib.sha1).digest()[:16]

print("PMKID:", pmkid.hex())
```

### Tahap 4 - Membandingkan PMKID

Setelah menghitung PMKID dari berbagai kandidat password yang ada di wordlist, PMKID tersebut dibandingkan dengan PMKID asli yang berhasil di-capture oleh attacker. Apabila hasilnya sama, maka password tersebut benar.

Jika hasilnya tidak sama, maka attacker mengulang kembali dari Tahap 2 sampai Tahap 4 dengan kandidat password berikutnya yang ada di wordlist sampai ditemukan kecocokan atau seluruh kandidat password di wordlist habis dicoba.
