# kubernetes-solr
Kubernetes resource for Apache Solr.

version: 5.5

部署方式：
1. 独立部署-standlone 
2. 集群部署-cloud

## 独立部署

### 1. Clone kubernetes-solr

```sh
$ git clone git@172.16.60.11:rooot/examples.git
cd examples/catalog/solr
```
  如果是基于部署好的集群，部署文件已经内置在master节点上：
  
```
cd /opt/example_kubernetes/catalog/solr
```

### 2. Start Solr service

```sh
cd /opt/example_kubernetes/catalog/solr 
or
cd examples/catalog/solr
$ kubectl create -f solr-service.yaml
service "solr-service" created
```


### 3 Start Solr controller

```sh
$ kubectl create -f solr-controller.yaml
deployment "solr-controller" created
```


### 4. Check Solr pod

```sh
$ kubectl get pods
NAME                    READY     STATUS    RESTARTS   AGE
solr-controller-r15v3   1/1       running   0          2m
```

### 5. Open Solr Admin UI in a browser

```sh
http://masterip:30059/solr/#/
```

Open Solr Admin UI in a browser.



## 集群部署

### 1. Start Zookeeper ensemble

请参见zookeeper部署

### 2. Clone kubernetes-solr

参见独立部署第1步

### 3. Start Solr services

参见独立部署第2步

### 4. Check Solr servicers

参见独立部署第3步

### 5. Start Solr controllers

```sh
$ kubectl create -f solrcloud-controller.yaml
Deployment "solr-controller" created
```

### 6. Check Solr pods

```sh
$ kubectl get pods -l app=solr-pod -o wide
NAME                    READY     STATUS    RESTARTS   AGE       NODE
solr-controller-2hqzq   1/1       Running   0          9s        172.17.4.201
solr-controller-rzjuw   1/1       Running   0          9s        172.17.4.201
```


### 7. Open Solr Admin UI in a browser

```sh
http://masterip:30059/solr/#/
```

Open Solr Admin UI in a browser.

### 10. Scale Solr pods

```sh
$ kubectl scale --replicas=3 Deployment solr-controller
Deployment "zookeeper-controller" scaled

$ kubectl scale --replicas=6 Deployment solr-controller
Deployment "zookeeper-controller" scaled

$ kubectl get pods -l app=zookeeper-pod -o wide
NAME                         READY     STATUS    RESTARTS   AGE       NODE
zookeeper-controller-li1sa   1/1       Running   0          7s        172.17.4.201
zookeeper-controller-o94xj   1/1       Running   0          2m        172.17.4.201
zookeeper-controller-zbkuz   1/1       Running   0          54s       172.17.4.201
```



