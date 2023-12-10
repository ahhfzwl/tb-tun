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

创建自启动脚本

nano /etc/init.d/ipv6tb

#!/bin/sh
case "$1" in
  start)
    echo "Starting ipv6tb"
      setsid /usr/bin/tb_userspace tb 74.82.46.6 10.38.185.90 sit > /dev/null 2>&1 &
      sleep 1s
      ifconfig tb up
      ifconfig tb inet6 add 2001:470:23:607::2/64
      ifconfig tb mtu 1480
      route -A inet6 add ::/0 dev tb
      route -A inet6 del ::/0 dev venet0
    ;;
  stop)
    echo "Stopping ipv6tb"
      ifconfig tb down
      route -A inet6 del ::/0 dev tb
      killall tb_userspace
    ;;
  *)
    echo "Usage: /etc/init.d/ipv6tb {start|stop}"
    exit 1
    ;;
esac
exit 0

