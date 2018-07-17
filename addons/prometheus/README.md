到 k8s master 主机，执行：

cd /opt/example_kubernetes/addons/prometheus/

部署promethus
创建promethus POD到master节点：kubectl apply -f prometheus-deployment.yaml
检查：访问masterip:30000

