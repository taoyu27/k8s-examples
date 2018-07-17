到 k8s master 主机，执行：

cd /opt/example_kubernetes/addons/prometheus/

部署promethus
根据部署node-export,ceph-export,blackbox-export的节点IP修改prometheus-config-map.yaml 
创建configmap文件：kubectl apply -f prometheus-config-map.yaml
创建promethus POD到master节点：kubectl apply -f prometheus-deployment.yaml
创建nodeport: kubectl apply -f prometheus-service.yaml
检查：访问masterip:30000

部署node-exporter
kubectl apply -f node-exporter.yaml
检查：访问nodeip:9100

部署blackbox-exporter
在master创建/etc/blackbox_exporter/目录并copy文件blackbox_exporter_conf.yml到目录下
创建POD：kubectl apply -f blackbox-exporter.yaml
检查：访问masterip:9115


部署ceph-exporter
在master创建/etc/ceph/目录并copy需要监控的ceph集群配置文件ceph.conf到目录下
创建POD: kubectl apply -f ceph-export.yml
检查：访问masterip:9128
