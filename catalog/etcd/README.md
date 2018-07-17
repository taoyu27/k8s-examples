# ETCD Cluster

> **Note**, 此版本采用 CoresOS Etcd Operator (https://github.com/coreos/etcd-operator), 目前项目状态为 **Beta** 版本

## Requirements

> （2018-05-30）目前官方版本最后镜像为0.9.2，dev 版本包含可以指定初始化 busybox 镜像

* etcd operator 运行在 Kubernetes 1.8+.
* etcd 3.2.13+
* images:
    * hub.chinacloud.com/kubernetes/etcd-operator:v0.9.3
    * hub.chinacloud.com/catalog/etcd:3.2.13
    * hub.chinacloud.com/common/busybox/1.28.0-glibc


## Deploy etcd operator

### Create etcd operator

1. 创建 etcd operator

```
kubectl create -f /opt/example_kubernetes/catalog/etcd/etcd-operator.yaml
```

2. 查看运行状态

```
kubectl get crd

NAME                                          AGE
etcdclusters.etcd.database.coreos.com         1d
``` 

### uninstall etcd operator 

```
kubectl delete -f /opt/example_kubernetes/catalog/etcd/etcd-operator.yaml
kubectl delete endpoints etcd-operator
kubectl delete crd etcdclusters.etcd.database.coreos.com
kubectl delete clusterrole etcd-operator
kubectl delete clusterrolebinding etcd-operator
```

##  etcd cluster

### Create etcd cluster

1. 执行创建命令

```
kubectl create -f /opt/example_kubernetes/catalog/etcd/etcd-cluster.yaml
```

2. 查看创建结果

```
kubectl get po -n <your_namespace>

NAME                               READY           STATUS    RESTARTS    AGE
example-etcd-cluster-6jqsffmqj5    1/1             Running   0           1d
example-etcd-cluster-ntkx9gs7hm    1/1             Running   0           1d
example-etcd-cluster-rbx2fcdfgj    1/1             Running   0           1d
```
