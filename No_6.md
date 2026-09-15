# Langkah-Langkah

Masuk ke root@Mika. Terus bikin file dengan command
```bash
nano traffic_protocol7.sh
```
isi file ini dengan file yang diberikan di gdocs. 

Sekarang buka aplikasi GNS3 (HARUS APLIKASI)
Klik kanan di link yang menghubungkan Mika dan Switch 1. 
Pilih Start Capture. Kemudian setelah Wireshark terbuka, kembali ke terminal Mika.

Sekarang, jalanin:
```bash
chmod +x traffic_protocol7.sh
./traffic_protocol7.sh
```

Buat cari data di Wireshark, filter data yang masuk dengan `dns || icmp`.

Screenshots hasil:
![alt text](assets/wire6_1.png)
![alt text](assets/wire6_2.png)
