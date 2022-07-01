# k8s 安装步骤

准备三台机器
192.168.56.100 master
192.168.56.101 node-1
192.168.56.102 node-2

1.关闭防火墙

```bash
#停止防火墙
$ systemctl stop firewalld
#永久关闭防火墙
$ systemctl disable firewalld
#查看防火墙状态
$ systemctl status firewalld
```

2.关闭selinux，编辑/etc/selinux/config文件，改成SELINUX=disabled

```bash
$ vi /etc/selinux/config
# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
SELINUX=disabled
# SELINUXTYPE= can take one of three values:
#     targeted - Targeted processes are protected,
#     minimum - Modification of targeted policy. Only selected processes are protected.
#     mls - Multi Level Security protection.
SELINUXTYPE=targeted
```

3.关闭swap，编辑/etc/fstab，注释掉swap那一行

```bash

#临时关闭
$ swapoff -a

#永久关闭
$ vim /etc/fstab
#
# /etc/fstab
# Created by anaconda on Tue May 31 12:02:25 2022
#
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
/dev/mapper/centos-root /                       xfs     defaults        0 0
UUID=0657fe31-0528-4852-be66-22def098d384 /boot                   xfs     defaults        0 0
#/dev/mapper/centos-swap swap                    swap    defaults        0 0

$ free #查看swap是否为0
              total        used        free      shared  buff/cache   available
Mem:        4027632     1066868     1993456       11348      967308     2792788
Swap:             0           0           0

```

4.同步时间

```bash
#下载时钟同步器
$ yum install ntpdate
$ ntpdate cn.pool.ntp.org
```

5.配置hosts

```bash
$ vim /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6

192.168.56.100 master
192.168.56.101 node-1
192.168.56.102 node-2
192.168.56.99 origin

```

6.将桥接的IPV4流量传递到iptables的链上

```bash
$ cat > /etc/sysctl.d/k8s.conf << EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF

$ sysctl --system
```

7.安装docker

```bash
#下载并安装
$ wget https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo -O/etc/yum.repos.d/docker-ce.repo 
$ yum install docker-ce-20.10.2

#配置加速镜像
$ mkdir -p /etc/docker
$ tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["http://hub-mirror.c.163.com", "https://docker.mirrors.ustc.edu.cn"],
  "exec-opts": ["native.cgroupdriver=systemd"]
}
EOF

#设置开机自启动
$ systemctl enable docker
$ systemctl start docker

#查看是否成功
$ docker --version

```

8.添加阿里云yum软件源

```bash
$ cat > /etc/yum.repos.d/kubernetes.repo << EOF
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg
https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF
```

9.安装kubeadm、kubelet和kubectl。
在部署kubernetes时，要求master-node和worker-node上的版本保持一致，否则会出现版本不匹配导致奇怪的问题出现。

```bash
#安装kubeadm、kubelet和kubectl
$ yum install -y kubelet-1.20.1 kubectl-1.20.1 kubeadm-1.20.1 kubernetes-cni-1.20.1 --nogpgcheck
# 开机启动
$ systemctl enable kubelet
#如果出现错误提示，只需要清除yum缓存即可，然后再执行安装命令
$ yum clean all

```

以上9个步骤三台机器都要执行

10.部署kubernetes master

```bash
$ kubeadm init \
--apiserver-advertise-address=192.168.56.100 \
--image-repository registry.aliyuncs.com/google_containers \
--kubernetes-version v1.20.1 \
--service-cidr=10.1.0.0/16 \
--pod-network-cidr=10.244.0.0/16

#如果出现错误
$ kubeadm reset
$ rm -rf /etc/kubernetes
$ rm -rf /var/lib/etcd/

#成功了以后，复制如下命令直接执行
$ mkdir -p $HOME/.kube
$ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
$ sudo chown $(id -u):$(id -g) $HOME/.kube/config

#安装pod网络插件cni
$ kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
#查看pod是否都running
$ kubectl get pods -n kube-system
#查看node是否ready
$ kubectl get node
```

11.加入node节点

```bash
$ kubeadm join 192.168.56.100:6443 --v=5 --token u9rswh.4201x61jdm8owxme \
    --discovery-token-ca-cert-hash sha256:480bf0f547f4aa6dddf1972d0c8e14bfdb108e572fd3f6230914977bd64e407b
#如果有报错，可以查看日志，解决完以后再次执行该命令
$ kubeadm reset
$ rm -rf /etc/kubernetes
$ rm -rf /var/lib/etcd/

```

文档转载自：<https://luckymrwang.github.io/2021/04/25/CentOS7-%E9%83%A8%E7%BD%B2K8S%E9%9B%86%E7%BE%A4/>
