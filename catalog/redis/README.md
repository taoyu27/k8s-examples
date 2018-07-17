### redis安装步骤
#### 前置条件

* Kubernetes 1.4+ with Beta APIs enabled
* PV provisioner support in the underlying infrastructure
* helm installed

#### 安装Redis chart

1. 根据需求修改values.yaml文件，比如，image地址；是否启用password；是否使用持久化存储；resources资源限制等

2. 获取charts文件
    ```
    git clone git@172.16.60.11:rooot/examples.git 
    
    ```
3. 部署

    ```
    $ helm install --name my-release examples/catalog/redis

    ```

4. 高级部署

    安装的时候可以声明参数
	
	```
	helm install --name my-release examples/catalog/redis
	```
    
    如， 安装的时候设置密码

    ```
    $ helm install --name my-release --set redisPassword={{secretpassword}} examples/catalog/redis
    ```
    
    如， 安装的时候制定已存在的持久化存储
    
    ```
    helm install --set persistence.existingClaim=PVC_NAME examples/catalog/redis
    ```
    
#### 访问


1.  查询redis的nodeport

        ```
          $[root@master ~]# kubectl get svc |grep redis
          redis-test-redis       NodePort    10.3.0.5     <none>        6379:30054/TCP   23m
        
        ```
    
2.  利用redis client访问
    
    **Host** 节点地址

    **Port**  30054
    
    **Auth**  {{redispassword}}
    
3.  集群中访问
    
    获取pod

        ```
        [root@master ~]# kubectl get pods |grep redis
        redis-test-redis-6cb97f6d86-9mz62           1/1       Running   0          31m
        ```
        
    访问redis pod
        
        ```
        kubectl exec -it pod_name bash
        ```
    访问数据库
        
        ```
        redis-cli -h redis-test-redis -a $REDIS_PASSWORD
        ```
    
    如果没有设置密码
            
        ```
        redis-cli -h redis-test-redis 
        ```

#### 参考

可配置参数请参考
https://github.com/kubernetes/charts/tree/master/stable/redis