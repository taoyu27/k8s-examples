# Redis Standalone Service

## Requirements

* Kubernetes 1.7+
* images:
    * hub.chinacloud.com/catalog/redis:3.2-alpine

## Create Redis Service

1. 创建

```
kubectl create -f /opt/example_kubernets/catalog/redis/standalone/redis-service.yml
```

2. 查看服务状态

```
kubectl get pods -o wide

NAME                                        READY     STATUS    RESTARTS   AGE
redis-5559798886-5jtqh                      1/1       Running   0          8m
```

```
kubectl get svc
NAME             TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)            AGE
redis            NodePort    10.3.0.169   <none>        6379:19502/TCP     11m
```

3. 使用 redis-cli 连接

```
redis-cli -h <pod_in_node_ip> -p 19502
> get name
> <nil>
> set name hello
> get name
> hello
```