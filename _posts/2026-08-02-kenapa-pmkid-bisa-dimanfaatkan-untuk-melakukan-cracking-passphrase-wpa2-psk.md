---
layout: post
title: "Kenapa PMKID Bisa Dimanfaatkan untuk Melakukan Cracking Passphrase WPA2-PSK?"
date: 2026-08-02 19:25:00 +0700
tags: [wifi-hacking, pmkid]
---

### Tahap 1 - Mendapatkan Data-Data yang Dibutuhkan

Untuk melakukan cracking passphrase WPA2-PSK menggunakan PMKID, attacker harus mengumpulkan data-data berikut:
- **PMKID**: Nilai hash yang berhasil di-capture dari EAPOL frame (M1)
- **SSID**: Nama Wi-Fi
- **MAC AP**: MAC address Access Point
- **MAC STA**: MAC address client
- **Wordlist**: File yang berisi kandidat password

> Data-data tersebut dapat diperoleh attacker dengan cara melakukan capture terhadap trafik Wi-Fi menggunakan tools seperti `wireshark` atau `airodump-ng`.

### Tahap 2 - Menghitung PMK

Setelah data-data tersebut berhasil dikumpulkan, attacker kemudian menghitung PMK menggunakan rumus berikut:

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

Setelah PMK berhasil didapat, attacker kemudian menghitung PMKID menggunakan rumus berikut:

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

Jika hasilnya tidak sama, attacker akan mengulang proses dari Tahap 2 hingga Tahap 4 menggunakan kandidat password berikutnya, hingga ditemukan PMKID yang sama atau seluruh kandidat password pada wordlist telah habis dicoba.

## Kesimpulan

PMKID dapat dimanfaatkan untuk melakukan cracking passphrase WPA2-PSK karena nilai PMKID dihasilkan dari kombinasi PMK, SSID, serta MAC address AP dan client, sehingga attacker dapat menghitung ulang PMKID dari setiap kandidat password di wordlist dan membandingkannya dengan PMKID asli yang berhasil di-capture.
