#### 步骤1 创建存储端
#选择一个k8s node节点，用docker命令启动ceph存储端
#备份/etc/ceph
[ -d /etc/ceph ] && cp -pr /etc/ceph "/etc/ceph.bak$(date +%s)"
/bin/rm -rf /etc/ceph

#MON_IP为k8s node节点的IP地址
#CEPH_PUBLIC_NETWORK为k8s node节点的IP子网
ceph_container=$(docker run -d --net=host -v /etc/ceph:/etc/ceph -e MON_IP=172.16.74.141 -e CEPH_PUBLIC_NETWORK=172.16.74.0/24 hub.chinacloud.com/library/ceph/demo:tag-build-stable-2.2.11-jewel-centos-7)
echo "CONTAINER ID: ${ceph_container}"

#查看存储ceph的监控状况
docker exec  ${ceph_container} ceph -s

#获取存储ceph的 admin key
docker exec  ${ceph_container} ceph auth get-key client.admin | base64
QVFEeUtrOWFnY2Y3QnhBQVNHTlNKcm9YT3Eyam1RekIySHBYV1E9PQ==

#创建存储ceph pool 和 client.kube key
docker exec  ${ceph_container} ceph osd pool create kube 64
docker exec  ${ceph_container} ceph auth add client.kube mon 'allow r' osd 'allow rwx pool=kube'

#获取存储ceph的 client.kube key
docker exec  ${ceph_container} ceph auth get-key client.kube | base64
QVFBaEswOWFOQ25SRkJBQXlrSUZTc0h6VDZrTWRPeUk3Vk53eUE9PQ==



#### 步骤2 在k8s master上，对yaml文件进行修改
cd /opt/example_kubernetes/addons/ceph-rbd/demo/deploy_docker_ceph_storage_only_for_develop

#替换 monitors IP地址为 CLUSTER-IP 
vim ./class.yaml
monitors: 172.16.74.141:6789

#替换 ceph-admin-secret key
#替换 ceph-secret key
vim secrets.yaml 
apiVersion: v1
kind: Secret
metadata:
  name: ceph-admin-secret
  namespace: kube-system
type: "kubernetes.io/rbd"
data:
  key: QVFEeUtrOWFnY2Y3QnhBQVNHTlNKcm9YT3Eyam1RekIySHBYV1E9PQ==
---
apiVersion: v1
kind: Secret
metadata:
  name: ceph-secret
type: "kubernetes.io/rbd"
data:
  key: QVFBaEswOWFOQ25SRkJBQXlrSUZTc0h6VDZrTWRPeUk3Vk53eUE9PQ==
  
#创建到ceph的连接和应用
kubectl create -f ./secrets.yaml \
 -f ./class.yaml \
 -f ./rbd-provisioner-demo.yaml \
 -f ./claim.yaml \
 -f ./helloworld_mnt_rbd.yaml
 
  secret "ceph-admin-secret" created
  secret "ceph-secret" created
  storageclass "rbd-demo" created
  deployment "rbd-provisioner-demo" created
  persistentvolumeclaim "claim-demo" created
  deployment "helloworld-mnt-rbd" created
 
#确认pod运行的结果
kubectl get pod -o wide

[环境清空]
#清空应用
kubectl delete -f ./helloworld_mnt_rbd.yaml \
-f ./claim.yaml

#清空存储配置
kubectl delete -f ./rbd-provisioner-demo.yaml \
-f ./class.yaml \
-f ./secrets.yaml 

