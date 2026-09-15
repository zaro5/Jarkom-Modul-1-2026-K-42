Petunjuk:
Masuk ke root@Lain

Jalanin: 
cat << 'EOF' > /root/cek_status.sh
#!/bin/bash

echo "DEBINET ROUTER 'LAIN' - STATUS CHECK"
echo ""

echo "1. Memastikan IP Forwarding Aktif..."
sysctl -w net.ipv4.ip_forward=1
echo ""

echo "2. Mengaktifkan Kembali Aturan NAT (MASQUERADE)..."
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
echo ""

echo "3. Ringkasan Interface & IP (ip -br a):"
ip -br a
echo ""

echo "4. Status Tabel NAT (iptables -t nat -L -v -n):"
iptables -t nat -L -v -n
echo ""

echo "VERIFIKASI SELESAI & SIAP BEROPERASI"
EOF


Jalanin:
chmod +x /root/cek_status.sh

Jalanin:
/root/cek_status.sh
