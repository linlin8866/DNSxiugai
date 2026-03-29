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




dns_list=(208.67.222.222 208.67.220.220 4.2.2.1 4.2.2.2 168.126.63.1 168.126.63.2 1.1.1.1 1.0.0.1 37.235.1.174 216.146.35.35 216.146.36.36 8.26.56.26 8.20.247.20 64.6.64.6 64.6.65.6 77.88.8.8 77.88.8.1 80.80.80.80 80.80.81.81 94.140.14.14 94.140.15.15 156.154.70.1 156.154.70.5 156.154.71.1 156.154.71.5 103.197.104.178 103.197.106.75 203.189.136.148 203.112.2.4 84.200.69.80 84.200.70.40 195.46.39.39 195.46.39.40 109.69.8.51 91.239.100.100 89.233.43.71 81.218.119.11 209.88.198.133 185.222.222.222 45.11.45.11 74.82.42.42 66.220.18.42 104.236.210.29 45.55.155.25 185.228.169.8 185.228.169.9 185.228.169.10 185.228.169.11 185.228.168.18 185.228.169.168 202.79.32.33 202.79.32.34 194.242.2.2 194.242.2.3 194.242.2.4 194.242.2.9); echo "=== DNS 测速结果 ==="; for dns in "${dns_list[@]}"; do res=$(ping -c2 -W2 "$dns" 2>/dev/null | awk '/time=/ {print $7}' | cut -d= -f2 | head -n1); if [ -n "$res" ]; then echo "$dns $res"; else echo "$dns timeout"; fi done | sort -k2n






ipv6_dns_list=(
2001:4860:4860::8888
2001:4860:4860::8844
2606:4700:4700::1111
2606:4700:4700::1001
2a00:5a60::ad1:0ff
2a00:5a60::ad2:0ff
2a00:5a60::bad1:0ff
12a00:5a60::bad2:0ff
2001:1608:10:25::1c04:b12f
2001:1608:10:25::9249:d69b
2a00:1508:0:4::9
2620:119:35::35
2620:119:53::53
2610:a1:1018::1
2610:a1:1019::1
2620:fe::fe
2620:fe::9
2620:74:1b::1:1
2620:74:1c::2:2
2001:67c:28a4::
2a01:3a0:53:53::
2001:418:3ff::53
2001:418:3ff::153
2a0d:2a00:1::2
2a0d:2a00:2::2
2a0d:2a00:1::1
2a0d:2a00:2::1
2a0d:2a00:1::
2a0d:2a00:2::
)
echo "=== IPv6 DNS 测速结果（延迟ms，越低越好） ==="
echo "----------------------------------------------"
for dns in "${ipv6_dns_list[@]}"; do
  res=$(ping6 -c2 -W2 "$dns" 2>/dev/null | awk '/time=/ {print $7}' | cut -d= -f2 | head -n1)
  if [ -n "$res" ]; then
    echo "$dns $res"
  else
    echo "$dns timeout"
  fi
done | sort -k2n
