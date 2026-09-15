telnet 10.4.89.247 5060 
#Masuk ke Alice

# 1. Nyalakan interface eth0
ip link set dev eth0 up

# 2. Pasang IP Statis Alice (Subnet Switch 1)
ip addr add 192.168.1.2/24 dev eth0

# 3. Arahkan Default Gateway ke router Lain
ip route add default via 192.168.1.1 dev eth0

# 4. Set DNS Resolver Google
echo "nameserver 8.8.8.8" > /etc/resolv.conf


Lakukan ke sisanya:

Chisa
telnet 10.4.89.247 5062
ip link set dev eth0 up
ip addr add 192.168.2.2/24 dev eth0
ip route add default via 192.168.2.1 dev eth0
echo "nameserver 8.8.8.8" > /etc/resolv.conf

Eiri
telnet 10.4.89.247 5075
ip link set dev eth0 up
ip addr add 192.168.3.3/24 dev eth0
ip route add default via 192.168.3.1 dev eth0
echo "nameserver 8.8.8.8" > /etc/resolv.conf

Knights
telnet 10.4.89.247 5063
ip link set dev eth0 up
ip addr add 192.168.3.2/24 dev eth0
ip route add default via 192.168.3.1 dev eth0
echo "nameserver 8.8.8.8" > /etc/resolv.conf

Mika
telnet 10.4.89.247 5061
ip link set dev eth0 up
ip addr add 192.168.1.3/24 dev eth0
ip route add default via 192.168.1.1 dev eth0
echo "nameserver 8.8.8.8" > /etc/resolv.conf

Note:
check it with 
```bash
    ping -c 3 8.8.8.8
```


(DI SERVER LAIN)
root@Lain:~# ip addr add 192.168.3.1/24 dev eth3
ip link set dev eth3 up
Error: ipv4: Address already assigned.
root@Lain:~# # 1. Pastikan IP forwarding aktif
sysctl -w net.ipv4.ip_forward=1

# 2. Hapus aturan NAT lama yang mungkin kurang pas, lalu buat ulang yang bersih
iptables -t nat -F
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

# 3. (Opsional tapi aman) Pastikan aturan forward firewall mengizinkan lalu lintas antar interface
iptables -P FORWARD ACCEPT
net.ipv4.ip_forward = 1