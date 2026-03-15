cat /etc/resolv.conf



chattr -i /etc/resolv.conf
systemctl stop systemd-resolved
systemctl disable systemd-resolved
rm -f /etc/resolv.conf
cat > /etc/resolv.conf <<EOF
nameserver 208.67.222.222
nameserver 1.1.1.1
nameserver 8.8.4.4
nameserver 8.8.8.8
nameserver 2606:4700:4700::1111
nameserver 2606:4700:4700::1111
nameserver 2606:4700:4700::1111
nameserver 2606:4700:4700::1111
EOF
chattr +i /etc/resolv.conf
echo "✅ DNS 已永久设置，重启不变"
cat /etc/resolv.conf




chattr -i /etc/resolv.conf
systemctl stop systemd-resolved
systemctl disable systemd-resolved
rm -f /etc/resolv.conf
cat > /etc/resolv.conf <<EOF
nameserver 1.1.1.1
nameserver 1.0.0.1
nameserver 2606:4700:4700::1111
nameserver 2606:4700:4700::1001
EOF
chattr +i /etc/resolv.conf
echo "DNS 已永久锁定 ✅"
cat /etc/resolv.conf





chattr -i /etc/resolv.conf
systemctl stop systemd-resolved
systemctl disable systemd-resolved
rm -rf /etc/resolv.conf
cat > /etc/resolv.conf << EOF
nameserver 1.1.1.1
nameserver 1.0.0.1
nameserver 2606:4700:4700::1111
nameserver 2606:4700:4700::1001
EOF
chattr +i /etc/resolv.conf
echo "====================================="
echo "✅ DNS 已永久设置，重启不会被覆盖！"
echo "====================================="
cat /etc/resolv.conf




systemctl stop systemd-resolved
systemctl disable systemd-resolved
rm -f /etc/resolv.conf
cat > /etc/resolv.conf << EOF
nameserver 1.1.1.1
nameserver 1.0.0.1
nameserver 2606:4700:4700::1111
nameserver 2606:4700:4700::1001
EOF
chattr +i /etc/resolv.conf
echo "✅ 自定义 DNS 已永久锁定，重启 VPS 依然生效"
cat /etc/resolv.conf



