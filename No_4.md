# Langkah-Langkah

## Masuk ke Alice
```bash
    telnet 10.4.89.247 5060 #Ini telnet nya Alice. Kalo error, coba cekk aja di GNS3 nya, siapa tau port nya ganti
```

Kalo udah, nyalain interface eth0
```bash
ip link set dev eth0 up

# Lanjut pasang IP Statis Alice (Subnet Switch 1)
ip addr add 192.168.1.2/24 dev eth0

# Arahkan Default Gateway ke router Lain
ip route add default via 192.168.1.1 dev eth0

# Set DNS Resolver Google
echo "nameserver 8.8.8.8" > /etc/resolv.conf
```

## Lanjut ke Container sisanya

Lakukan ke sisanya:
Jangan lupa masuk ke container mereka pake telnet <IP + Port>

Buat ke Chisa
```bash 
telnet 10.4.89.247 5062
ip link set dev eth0 up
ip addr add 192.168.2.2/24 dev eth0
ip route add default via 192.168.2.1 dev eth0
echo "nameserver 8.8.8.8" > /etc/resolv.conf
```

Eiri
```bash 
telnet 10.4.89.247 5075
ip link set dev eth0 up
ip addr add 192.168.3.3/24 dev eth0
ip route add default via 192.168.3.1 dev eth0
echo "nameserver 8.8.8.8" > /etc/resolv.conf
```

Knights
```bash
telnet 10.4.89.247 5063
ip link set dev eth0 up
ip addr add 192.168.3.2/24 dev eth0
ip route add default via 192.168.3.1 dev eth0
echo "nameserver 8.8.8.8" > /etc/resolv.conf
```

Mika (masuk ke nano /etc/network/interfaces)
```bash
# Static config for eth0
auto eth0
iface eth0 inet static
        address 192.232.1.3
        netmask 255.255.255.0
        gateway 192.232.1.1
        up echo nameserver 192.232.1.1 > /etc/resolv.conf
```

```bash
telnet 10.4.89.247 5061
ip link set dev eth0 up
ip addr add 192.168.1.3/24 dev eth0
ip route add default via 192.168.1.1 dev eth0
echo "nameserver 8.8.8.8" > /etc/resolv.conf
```

Note:
check it with 
```bash
    ping -c 3 8.8.8.8
```

Jalankan ini juga di root@Lain untuk nyalain interface
```bash
ip link set dev eth0 up
ip link set dev eth1 up
ip link set dev eth2 up
ip link set dev eth3 up

#ketik ip-br a untuk checking
```
Pasang IP Statis kalo udah
```bash
ip addr add 192.168.1.1/24 dev eth1
ip addr add 192.168.2.1/24 dev eth2
ip addr add 192.168.3.1/24 dev eth3
```

## Pembuktian

Harusnya ini udah bisa untuk pembuktian
uji coba di masing2 container (client)
```bash
ping -c 2 8.8.8.8
ping -c 2 google.com
```

## NOTE
Kalau terjadi seperti  ini
```bash
root@Mika:~# ping -c 2 google.com
ping: google.com: Temporary failure in name resolution
```
Jalankan:
```bash
echo "nameserver 8.8.8.8" > /etc/resolv.conf
```

Kalo udah bisa, lanjut nomor 5