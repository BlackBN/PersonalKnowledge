# k8s 命令

``` bash
#查询所有api资源对象
$ kubectl api-resources 

#查询api的版本
$ kubectl api-versions 

#查看某个资源名对象名有哪些字段
$ kubectl explain <资源名对象名> --recursive 

#查看具体字段
$ kubectl explain svc.spec.ports 
$ kubectl explain pod.spec.containers

#显示集群信息
$ kubectl cluster-info  

#使用 kubectl config 组织集群访问
$ kubectl config view
$ kubectl config set-cluster mycluster --server="https://192.168.56.100:6443" --certificate-authority=k8s-ca/ca.crt --embed-certs=true --kubeconfig=k8s-ca/myconfig
$ kubectl config set-credentials xujiayu --client-certificate=k8s-ca/xujiayu.crt --client-key=k8s-ca/xujiayu.key --embed-certs=true  --kubeconfig=k8s-ca/myconfig
$ kubectl config set-context xujiayu@mycluster --cluster=mycluster --user=xujiayu --kubeconfig=k8s-ca/myconfig
$ kubectl config use-context xujiayu@mycluster --kubeconfig=k8s-ca/myconfig
$ kubectl get pods --kubeconfig=k8s-ca/myconfig
```
