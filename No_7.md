# Langkah-Langkah

## Install user + pwdnya
Masuk ke root@Chisa

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


## Instalasi vsftpd (Kalo dari Kali Linux)

Masuk ke Kali Linux (bukan root@Chisa)
```bash
cd ~
sudo apt update
sudo apt download vsftpd ssl-cert
```
Cek dan ubah file jadi base64
```bash
ls -l vsftpd*.deb ssl-cert*.deb
base64 vsftpd_*_amd64.deb #Bakal keluar text panjang, copy semua dan simpen
```

Sekarang pindah ke container root@Chisa
```bash
cd /root/
nano vsftpd.txt
```
paste yang udah di copy tadi, terus simpen file vsftpd.txt nya

Lanjut, ubah filenya
```bash
base64 -d vsftpd.txt > vsftpd_3.0.5-0.7_amd64.deb
dpkg -i vsftpd_3.0.5-0.7_amd64.deb
```

## Lanjut Pengerjaan TT

Tetep aja di root@Chisa
```bash
# 1. Buat folder shared directory
mkdir -p /var/wired/data

# 2. Buat user alice, mika, dan eiri jika belum ada (set password masing-masing)
useradd -m -d /var/wired/data alice && passwd alice
useradd -m -d /var/wired/data mika && passwd mika
useradd -m -d /var/wired/data eiri && passwd eiri
```
Nanti harusnya tulisannya: user udah ada. Enter aja. 

Bikin file list user yang di blockir
```bash
echo "eiri" >> /etc/vsftpd.user_list
```

config vsftpd
```bash
cat << 'EOF' > /etc/vsftpd.conf
listen=YES
anonymous_enable=NO
local_enable=YES
write_enable=YES
local_umask=022
dirmessage_enable=YES
use_localtime=YES
connect_from_port_20=YES
chroot_local_user=YES
allow_writeable_chroot=YES
secure_chroot_dir=/var/run/vsftpd/empty
pam_service_name=vsftpd

# Konfigurasi Blacklist (Eiri)
userlist_enable=YES
userlist_file=/etc/vsftpd.user_list
userlist_deny=YES

# Konfigurasi Konfigurasi Per-User (Alice & Mika)
user_config_dir=/etc/vsftpd_user_conf
EOF
```

Sekarang bikin direktori config/user dan masukin file2nya

```bash
mkdir -p /etc/vsftpd_user_conf
echo "write_enable=YES" > /etc/vsftpd_user_conf/alice
echo "write_enable=NO" > /etc/vsftpd_user_conf/mika
```

Jalanin vsftpd di background
```bash
mkdir -p /var/run/vsftpd/empty
vsftpd /etc/vsftpd.conf &
```

Sekarang uji coba
Karena nanti dia bakal denied kalo langsung dicoba, kita kasih izin tulis ke user Alice
```bash
chmod 777 /var/wired/data
```

Bikin file sebagai Alice
```bash
su - alice -c "touch /var/wired/data/signal_alice.txt"
ls -l /var/wired/data/signal_alice.txt #Ini untuk verifikasi
```