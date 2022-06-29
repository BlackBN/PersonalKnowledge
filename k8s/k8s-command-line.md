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

```
