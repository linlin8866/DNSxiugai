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





#!/bin/bash

# 1. 解锁 resolv.conf（新系统一般没锁，先做兼容）
chattr -i /etc/resolv.conf 2>/dev/null

# 2. 停止并禁用 systemd-resolved（温和版，不 mask，避免断网）
systemctl stop systemd-resolved
systemctl disable systemd-resolved

# 3. 删除旧配置，写入你要的 DNS 服务器
rm -f /etc/resolv.conf
cat > /etc/resolv.conf <<EOF
nameserver 95.85.95.85
nameserver 2.56.220.2
nameserver 2a03:90c0:999d::1
nameserver 2a03:90c0:9992::1
EOF

# 4. 锁定文件，禁止任何程序修改（包括系统服务）
chattr +i /etc/resolv.conf

# 5. 防止 NetworkManager 覆盖（如果系统安装了 NetworkManager）
if command -v nmcli &> /dev/null; then
    mkdir -p /etc/NetworkManager/conf.d
    cat > /etc/NetworkManager/conf.d/90-dns-none.conf <<EOF
[main]
dns=none
rc-manager=unmanaged
EOF
    systemctl reload NetworkManager
fi

# 6. 验证结果
echo -e "\n✅ DNS 已永久锁定，当前配置："
cat /etc/resolv.conf
echo -e "\n🔍 测试解析："
ping -c 2 google.com

