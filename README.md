1、通过命令cat /dev/net/tun返回File descriptor in bad state则说明tun/tap设备已经打开，否则滚蛋。

apt -y install iproute2 gcc git

cd /root

git clone https://github.com/acgrid/tb-tun.git

cd tb-tun

gcc tb_userspace.c -l pthread -o tb_userspace

mv tb_userspace /usr/bin/

wget -O /etc/init.d/ipv6tb https://raw.githubusercontent.com/ahhfzwl/tb-tun/master/ipv6tb

nano /etc/init.d/ipv6tb #替换脚本中的3个IP地址

chmod 755 /etc/init.d/ipv6tb

update-rc.d ipv6tb defaults

/etc/init.d/ipv6tb start
