# Langkah-Langkah

Masuk ke root@Chisa

Instalasi vsftpd dulu jangan lupa. (INI YANG GAK BISA ANJG)

Jalnkan:
```bash
mkdir -p /var/wired/data

useradd -m -d /var/wired/data -s /usr/sbin/nologin alice
useradd -m -d /var/wired/data -s /usr/sbin/nologin mika
useradd -m -d /var/wired/data -s /usr/sbin/nologin eiri
```

kemudian masukkan pwd:
```bash
echo "alice:password123" | chpasswd
echo "mika:password123" | chpasswd
echo "eiri:password123" | chpasswd
```
JANCOK