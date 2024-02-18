## Overview

由于各个Linux 发行版的网络管理工具是不同的，所以配置的方式也有差别。

所以只记录必须要背会的配置和常识。以及需要知道什么（Questions）


## Questions

```
Q： 这个 Linux 发行版的网络配置工具是？

Q：如何配置 `static IP`

Q：如何查看公网IP地址，如何确认自己是否在 运营商 `NAT` 环境中。

Q：如何
```



Q： 服务器环境常用的配置网络的方式有哪些？

1. **手动配置文件**。

2. **NetworkManager**

   `nmtui` ：文本配置界面（Text User Interface） 

   `nmcli`：命令行界面

3. **`systemd-networkd`**

   对于使用 systemd 的系统中

4. **命令行工具**

   `ifconfig`   `ping` 等。



#### 配置文件

网络配置文件

- 在 Red Hat/CentOS系统中：

   `/etc/sysconfig/network-scripts/ifcfg-<interface>` 文件（其中 `<interface>` 是您的网络接口名称，比如 `eth0`）。

- 在 Debian/Ubuntu 系统中：

   `/etc/network/interfaces` 

  

#### NetworkManager

`NetworkManager` ：在现代桌面 Linux 发行版中广泛使用的网络管理工具。包含了一个有文本用户界面（Text User Interface）的网络配置工具 `nmtui`。

```shell
sudo nmtui
```

 `NetworkManager` 的 nmtui 需要提前安装（包名：`NetworkManager-tui` 



#### 命令行工具

##### ifconfig

【英文原意】： configure a network interface

【功能描述】：配置网络服务。

这个命令如今作用就是查看 `IP` 地址的信息。它的输出解释如下：

> 需要记住并且最主要的。

```shell
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
      网络接口的状态标志。例如，UP 表示接口处于激活状态。 最大传输单元，1500 是以太网的标准 MTU。
        inet 172.24.95.132  netmask 255.255.240.0  broadcast 172.24.95.255
        IP 地址（子网）         子网掩码                 广播地址
        ether 00:16:3e:1c:91:da  txqueuelen 1000  (Ethernet)
        MAC地址
        RX packets 96093  bytes 131731706 (125.6 MiB)
        接收 包     数量     大小
        RX errors 0  dropped 0  overruns 0  frame 0
 Receive接收 错误     丢失        缓冲区溢出    
        TX packets 15364  bytes 1175399 (1.1 MiB)
Transmit发送 数据包  数量    大小
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
        发送 错误      丢失       缓冲区溢出              冲突
lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

```

eth0、lo ： 网络接口的名称

`eth` 是 "Ethernet" 的缩写，而数字 `0` 表示这是第一个（或主要的）以太网接口。在多个网络接口的系统中，每个额外的以太网接口将被依次命名为 `eth1`、`eth2`、`eth3` 等。

`lo` ： 本地回环网卡。



##### ping

```shell
ping optionn IP
  
  -b Allow pinging a broadcast address.（ 广播地址，用于对整个IP网段进行探测）
  -c count  指定ping的次数
  -s bytes（packetsize）
```

【功能描述】：向网络主机发送 `IMCP` 请求。



##### netstat

netstat 是网络查看的命令，用户查看本地开启端口，网络连接的信息。

可能需要安装（net-samp、net-tools）

```
netstat option

option:
        -r, --route              display routing table
        -n, --numeric            don't resolve names (使用 IP 和端口号显示，不使用域名与主机名)
        -p, --programs           display PID/Program name for sockets （PID/程序名）
        -l, --listening          display listening server sockets （ 仅显示监听状态下的连接）
        -t                       显示使用TCP协议的连接状况
        -u                       显示使用UDP协议的连接状况
        -a, --all                display all sockets (default: connected)
```

输出表头:

这些列的含义和值参考 man 文档。

```
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State  
tcp        0      0 0.0.0.0:ssh             0.0.0.0:*               LISTEN 

Active UNIX domain sockets (servers and established)
Proto RefCnt Flags       Type       State         I-Node   Path
unix  2      [ ACC ]     STREAM     LISTENING     7432     /run/systemd/journal/stdout
```

【snippet】：
查看指定的端口是否被监听：

```shell
netstat -nltup | grep {port}
```



Q: 查看所在的公网的 `IP` 地址

```shell
curl ifconfig.me
curl ipinfo.io
```



Q：如何配置 `静态IP` 地址。

配置局域网的IP地址，主要是通过 DHCP 客户端。不同的Linux 发行版的 DHCP的客户端不同。



树莓派： 配置文件 `/etc/dhcpcd.conf ` 中，

```
# static IPADRESS

# wlan 无线网络接口 eth0 以太网接口
interface wlan0   
# inform 动态获取网络其他的配置（DNS服务器，子网掩码...
inform=192.168.0.100
```



