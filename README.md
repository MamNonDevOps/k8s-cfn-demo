# k8s-cloudformation-demo

https://viblo.asia/p/k8s-basic-cai-dat-lab-kubernetes-phan-1-3kY4gWdy4Ae

In CICD Node
```
sudo yum install git -y
mkdir kubernetes_installation/
cd kubernetes_installation
git clone https://github.com/kubernetes-sigs/kubespray.git --branch release-2.16
cd kubespray
cp -rf inventory/sample inventory/z-cluster
cd inventory/z-cluster
```

Make a `hosts.yaml` in `z-cluster` folder
```
[all]
z-master1  ansible_host=172.31.0.11      ip=172.31.0.11
z-master2  ansible_host=172.31.0.12      ip=172.31.0.12
z-master3  ansible_host=172.31.0.13      ip=172.31.0.13
z-worker1  ansible_host=172.31.0.14      ip=172.31.0.14
z-worker2  ansible_host=172.31.0.15      ip=172.31.0.15
z-worker3  ansible_host=172.31.0.16      ip=172.31.0.16

[kube-master]
z-master1
z-master2
z-master3

[kube-node]
z-worker1
z-worker2
z-worker3

[etcd]
z-master1
z-master2
z-master3

[k8s-cluster:children]
kube-node
kube-master

[calico-rr]

[vault]
z-master1
z-master2
z-master3
z-worker1
z-worker2
z-worker3


sudo docker run --rm -it --mount type=bind,source=/home/ec2-user/kubernetes_installation/kubespray/inventory/z-cluster,dst=/inventory \
  --mount type=bind,source=/home/ec2-user/.ssh/id_rsa,dst=/root/.ssh/id_rsa \
  --mount type=bind,source=/home/ec2-user/.ssh/id_rsa,dst=/home/ec2-user/.ssh/id_rsa \
  quay.io/kubespray/kubespray bash


ansible-playbook -i /inventory/hosts.yaml cluster.yml --user=ec2-user --ask-pass --become --ask-become-pass


ssh-copy-id z-master1
ssh-copy-id z-master2
ssh-copy-id z-master3
ssh-copy-id z-worker1
ssh-copy-id z-worker2
ssh-copy-id z-worker3
```
