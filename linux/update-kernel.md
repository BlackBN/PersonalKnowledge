## 操作步骤：

### 小版本升级

1. 查看当前和可升级版本

```shell
[root@server-1 ~]# yum list kernel
Installed Packages
kernel.x86_64                           3.10.0-957.el7              @anaconda
Available Packages
kernel.x86_64                           3.10.0-957.5.1.el7          updates
```

2. 升级

```shell
[root@server-1 ~]# yum update kernel -y 
```

3. 重启并检查

```shell
[root@server-1 ~]# reboot 　　
[root@server-1 ~]# uname -r 
```

###  大版本升级

1. 载入公钥

```shell
[root@server-1 ~]# rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
```

2. 升级安装ELRepo

```shell
[root@server-1 ~]# rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-3.el7.elrepo.noarch.rpm
```

3. 载入elrepo-kernel元数据

```shell
[root@server-1 ~]# yum --disablerepo=\* --enablerepo=elrepo-kernel repolist
```

4. 查看可用的rpm包

```shell
[root@server-1 ~]# yum --disablerepo=\* --enablerepo=elrepo-kernel list kernel*
Installed Packages
kernel.x86_64                           3.10.0-957.el7                           @anaconda    
kernel.x86_64                           3.10.0-957.5.1.el7                       @updates     
kernel-tools.x86_64                     3.10.0-957.el7                           @anaconda    
kernel-tools-libs.x86_64                3.10.0-957.el7                           @anaconda    
Available Packages
kernel-lt.x86_64                        4.4.176-1.el7.elrepo                     elrepo-kernel
kernel-lt-devel.x86_64                  4.4.176-1.el7.elrepo                     elrepo-kernel
kernel-lt-doc.noarch                    4.4.176-1.el7.elrepo                     elrepo-kernel
kernel-lt-headers.x86_64                4.4.176-1.el7.elrepo                     elrepo-kernel
kernel-lt-tools.x86_64                  4.4.176-1.el7.elrepo                     elrepo-kernel
kernel-lt-tools-libs.x86_64             4.4.176-1.el7.elrepo                     elrepo-kernel
kernel-lt-tools-libs-devel.x86_64       4.4.176-1.el7.elrepo                     elrepo-kernel
kernel-ml.x86_64                        4.20.12-1.el7.elrepo                     elrepo-kernel　// 安装目标版本
kernel-ml-devel.x86_64                  4.20.12-1.el7.elrepo                     elrepo-kernel
kernel-ml-doc.noarch                    4.20.12-1.el7.elrepo                     elrepo-kernel
kernel-ml-headers.x86_64                4.20.12-1.el7.elrepo                     elrepo-kernel
kernel-ml-tools.x86_64                  4.20.12-1.el7.elrepo                     elrepo-kernel
kernel-ml-tools-libs.x86_64             4.20.12-1.el7.elrepo                     elrepo-kernel
kernel-ml-tools-libs-devel.x86_64       4.20.12-1.el7.elrepo                     elrepo-kernel
```

​	说明

​	lt  ：long term support，长期支持版本；

​	ml：mainline，主线版本；

5. 安装最新版本的kernel

```shell
[root@server-1 ~]# yum --disablerepo=\* --enablerepo=elrepo-kernel install  kernel-ml.x86_64  -y
```

6. 删除旧版本工具包

```shell
[root@server-1 ~]# yum remove kernel-tools-libs.x86_64 kernel-tools.x86_64  -y
```

7. 安装新版本工具包

```shell
[root@server-1 ~]# yum --disablerepo=\* --enablerepo=elrepo-kernel install kernel-ml-tools.x86_64  -y
```

8. 查看内核插入顺序

```shell
[root@server-1 ~]# awk -F \' '$1=="menuentry " {print i++ " : " $2}' /etc/grub2.cfg
0 : CentOS Linux (4.20.12-1.el7.elrepo.x86_64) 7 (Core)
1 : CentOS Linux (3.10.0-957.5.1.el7.x86_64) 7 (Core)
2 : CentOS Linux (3.10.0-957.el7.x86_64) 7 (Core)
3 : CentOS Linux (0-rescue-ca0f6fb3c5f24478abc0a2e275281d7a) 7 (Core)
```


说明：默认新内核是从头插入，默认启动顺序也是从0开始（当前顺序还未生效），或者使用：

```shell
[root@server-17 ~]# grep "^menuentry" /boot/grub2/grub.cfg | cut -d "'" -f2
CentOS Linux (4.20.12-1.el7.elrepo.x86_64) 7 (Core)
CentOS Linux (3.10.0-957.5.1.el7.x86_64) 7 (Core)
CentOS Linux (3.10.0-957.el7.x86_64) 7 (Core)
CentOS Linux (0-rescue-ca0f6fb3c5f24478abc0a2e275281d7a) 7 (Core)
```


其中文件 /etc/grub2.cfg 和 /boot/grub2/grub.cfg 内容一致。

9. 查看当前实际启动顺序

```shell
[root@server-1 ~]# grub2-editenv list
saved_entry=CentOS Linux (3.10.0-957.5.1.el7.x86_64) 7 (Core)
```

10. 设置默认启动

```shell
[root@server-1 ~]# grub2-set-default 'CentOS Linux (4.20.12-1.el7.elrepo.x86_64) 7 (Core)'
[root@server-1 ~]# grub2-editenv list
saved_entry=CentOS Linux (4.20.12-1.el7.elrepo.x86_64) 7 (Core)
```


或者直接设置数值

```shell
[root@server-1 ~]# grub2-set-default 0　　// 0代表当前第一行，也就是上面的4.20.12版本那一行内容
[root@server-1 ~]# grub2-editenv list
saved_entry=0
```

11. 重启并检查

```shell
[root@server-1 ~]# reboot 
[root@server-1 ~]# uname -r 
```

 
