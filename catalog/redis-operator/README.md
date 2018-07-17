# redis-operator

**NOTE**:  此版本借鉴github.com/spotahome/redis-operator, 且为alpha版本，只能作为测试环境使用

## Requirements
Redis Operator 运行在 Kubernetes 1.8+.


## Operator deployment on kubernetes

### Using a Deployment
To create the operator, you can directly create it with kubectl:

```
git clone git@172.16.60.11:rooot/examples.git
kubectl create -f examples/catalog/redis-operator/operator.yaml
```

Show Redis Operator

```
kubectl get pods 

NAME                                        READY     STATUS    RESTARTS   AGE
redisoperator-6d87f9fc57-pkvxl              1/1       Running   0          3d
```


## Usage
Once the operator is deployed inside a Kubernetes cluster, a new API will be accesible, so you'll be able to create, update and delete redisfailovers.

In order to deploy a new redis-failover a [specification](example/redisfailover.yaml) has to be created:

```
kubectl create -f examples/catalog/redis-operator/redisfailover.yaml
```

```
kubetl get pods
NAME                                        READY     STATUS    RESTARTS   AGE
rfr-redisfailover-0                         1/1       Running   0          6d
rfr-redisfailover-1                         1/1       Running   0          5d
rfr-redisfailover-2                         1/1       Running   0          5d
rfs-redisfailover-7595dcbdfb-7nc2m          1/1       Running   0          6d
rfs-redisfailover-7595dcbdfb-8qchs          1/1       Running   0          6d
rfs-redisfailover-7595dcbdfb-nw5d4          1/1       Running   0          6d
```

```
kubectl get svc
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)             AGE
rfs-redisfailover    ClusterIP   10.3.0.17    <none>        26379/TCP           6d
```

### Connection

一个用python-client连接redis集群的实例：

登录任一节点
1. 安装redis客户端
```
pip install redis
```
2. 登录并写入数据


test_connection.py

```

	from redis.sentinel import Sentinel
	sentinel = Sentinel([('localhost', 30088)], socket_timeout=0.1)
	master_url = sentinel.discover_master('mymaster')
	printf(master_url) #('10.244.1.72', 6379)
	slave_url = sentinel.discover_slaves('mymaster')
	printf(slave_url)  #[('10.244.2.78', 6380)]
	master = sentinel.master_for('mymaster', socket_timeout=0.1)
	slave = sentinel.slave_for('mymaster', socket_timeout=0.1)
	master.set('foo', 'bar')
	result = slave.get('foo')
	printf(result) #'bar'

```


## Cleanup
If you want to delete the operator from your Kubernetes cluster, the operator deployment should be deleted.

Also, the CRD has to be deleted too:
```
kubectl delete crd redisfailovers.storage.spotahome.com
```

# 编译方式

1. 获取源码
```
git clone https://github.com/spotahome/redis-operator.git
```
2. build可执行文件(最好科学上网，编译的时候设置selinux为disabled，不然会报错)
```
cd redis-operator
make build
```
3. build镜像
```
make image
```

# To Do
后端存储对接ceph或者nfs等持久化存储设备


