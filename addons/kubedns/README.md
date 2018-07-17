kube-dns

kube-dns schedules DNS Pods and Service on the cluster, other pods in cluster can use the DNS Service’s IP to resolve DNS names.

到 k8s master 主机，执行：

cd /opt/example_kubernetes/addons/kubedns/

kubectl apply -f kube-dns.yaml