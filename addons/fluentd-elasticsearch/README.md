1、使用镜像 、位置以及下载方法：

    elasticsearch:

    hub.chinacloud.com/catalog/elasticsearch:v5.6.4

    hub.chinacloud.com/catalog/alpine:3.6

    fluentd:

    hub.chinacloud.com/catalog/fluentd-elasticsearch:v2.0.2

    kibana:

   hub.chinacloud.com/catalog/kibana:v5.6.4

2、部署前置条件：

  a、k8s 以及 docker源已经部署好，并且正常运行。

  b、为了保证ES集群的正常运行，这里采用了statefulset来运行elasticsearch集群

  c、本例子采用的ES挂载的是emptydir， 即没有挂载存储卷来持久化ES数据，生产系统需要给ES挂载存储卷来使用；具体实现方法  参见在K8S上运行持久化ES集群（待研究）。

3、部署：

   a、部署es集群和服务，文件：es-statefulset.yaml  和 es-service.yaml

  实施部署命令：

kubectl apply -f  es-service.yaml

kubectl apply -f es-statefulset.yaml

b、部署fluentd的DaemonSet集群，文件：fluentd-es-ds.yaml和  fluentd-es-configmap.yaml


部署命令：

kubectl apply -f  fluentd-es-configmap.yaml
kubectl apply -f   fluentd-es-ds.yaml

c、部署kibana的deployment，文件：kibana-service.yaml和 kibana-deployment.yaml （非必须）


kubectl apply -f   kibana-service.yaml
kubectl apply -f   kibana-deployment.yaml


4、如果安装了kibana，可以通过kibana的界面来展示和搜索收集到的k8s容器日志

 访问方式： http://k8s_apiserver_ip:8080/api/v1/proxy/namespaces/kube-system/services/kibana-logging

