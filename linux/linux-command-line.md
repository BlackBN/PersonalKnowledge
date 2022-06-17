# linux 命令

```bash

$ echo $$ #打印当前进程PID, $$ 代表当前脚本的PID
$ ls -l /proc/$$/ns #查看当前进程的各个namespace
$ mount -t cgroup #确认cgroup为/sys/fs/cgroup
cgroup on /sys/fs/cgroup/systemd type cgroup (rw,nosuid,nodev,noexec,relatime,seclabel,xattr,release_agent=/usr/lib/systemd/systemd-cgroups-agent,name=systemd)
cgroup on /sys/fs/cgroup/freezer type cgroup (rw,nosuid,nodev,noexec,relatime,seclabel,freezer)
cgroup on /sys/fs/cgroup/cpuset type cgroup (rw,nosuid,nodev,noexec,relatime,seclabel,cpuset)
cgroup on /sys/fs/cgroup/perf_event type cgroup (rw,nosuid,nodev,noexec,relatime,seclabel,perf_event)
cgroup on /sys/fs/cgroup/cpu,cpuacct type cgroup (rw,nosuid,nodev,noexec,relatime,seclabel,cpuacct,cpu)
cgroup on /sys/fs/cgroup/hugetlb type cgroup (rw,nosuid,nodev,noexec,relatime,seclabel,hugetlb)
cgroup on /sys/fs/cgroup/net_cls,net_prio type cgroup (rw,nosuid,nodev,noexec,relatime,seclabel,net_prio,net_cls)
cgroup on /sys/fs/cgroup/blkio type cgroup (rw,nosuid,nodev,noexec,relatime,seclabel,blkio)
cgroup on /sys/fs/cgroup/devices type cgroup (rw,nosuid,nodev,noexec,relatime,seclabel,devices)
cgroup on /sys/fs/cgroup/pids type cgroup (rw,nosuid,nodev,noexec,relatime,seclabel,pids)
cgroup on /sys/fs/cgroup/memory type cgroup (rw,nosuid,nodev,noexec,relatime,seclabel,memory)


$ hostnamectl set-hostname node-1 #设置hostname
$ vim /etc/sysconfig/network-scripts/ifcfg-enp0s8 #设置网卡信息


```

## 网络命令

网络配置：ifconfig、ip
连通性探测：ping、traceroute、telnet、mtr
网络连接：netstat、ss、nc、lsof
流量统计：ifstat、sar、iftop
交换与路由：arp、arping、vconfig、route
防火墙：iptables、ipset
域名：host、nslookup、dig、whois
抓包：tcpdump
虚拟设备：tunctl、brctl、ovs

### 网络配置

最重要的两个工具就是 ifconfig 和 ip，这两个工具分别来自两个工具包 net-tools 和 iproute2，其中 net-tools 包还包含如 route、 netstat、 tc、 ifstat 等等常用的工具，不过，net-tools 包已经逐步在被 iproute2 包替换。

#### ifconfig

ifconfig 通常是用来查看网卡的信息（比如 IP 地址、收发包及丢包情况等），以及配置网卡（如启停网卡，修改网卡 MTU，修改 IP、MAC 地址等）。

1. 查看网卡信息：

```bash
$ ifconfig eth0 
```

2. 给网卡配置 IP 地址：

```bash
$ ifconfig eth0 192.168.1.10 
$ ifconfig eth0 192.168.1.10 netmask 255.255.255.0
$ ifconfig eht0 192.168.1.10 netmask 255.255.255.0  broadcast 192.168.1.255
```

3. 启停网卡：

```bash
$ ifconfig eth0 down
$ ifconfig eht0 up
```

#### ip


