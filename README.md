1 需要开启tun/tap
先检查tun/tap设备是否已经打开，可以通过命令cat /dev/net/tun检测，如果返回File descriptor in bad state，则说明tun/tap设备已经打开，否则需要到控制面板更改或给客服发ticket开启。

以root用户直接运行以下命令

apt -y install iproute2 gcc git

cd /root

git clone https://github.com/acgrid/tb-tun.git

cd tb-tun

gcc tb_userspace.c -l pthread -o tb_userspace

mv tb_userspace /usr/bin/

（简单解释下，安装依赖库，从github上获取源代码，编译，并将编译生成的tb_userspace移动到/etc目录下

下载自启动脚本

wget -O /etc/init.d/ipv6tb https://raw.githubusercontent.com/ahhfzwl/tb-tun/master/ipv6tb

替换脚本中的3个IP地址

nano /etc/init.d/ipv6tb

为该sh脚本添加可执行权限，以及自启动。

chmod 755 /etc/init.d/ipv6tb

update-rc.d ipv6tb defaults

手动启动

/etc/init.d/ipv6tb start
