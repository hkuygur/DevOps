# DevOps

#install vagrant and vagrant vmware tools
#run this command
vagrant up
#after running machines
#run this command
{
cat >> /etc/modules-load.d/containerd.conf <<EOF
overlay
br_netfilter
EOF
modprobe overlay
modprobe br_netfilter
}

{
cat >>/etc/sysctl.d/kubernetes.conf<<EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
EOF
sysctl --system
}

#Then, in master node:
kubeadm init
#this gives you a command for worker nodes join the master node
#check the node status
kubectl get nodes

#helm installing
curl -k https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash

#calico
curl https://docs.projectcalico.org/manifests/calico.yaml -O
kubectl apply -f calico.yaml

# metric
curl -k https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml -O
kubectl apply -f components.yaml

# nfs-subdir
helm repo add nfs-subdir-external-provisioner https://kubernetes-sigs.github.io/nfs-subdir-external-provisioner/ --insecure-skip-tls-verify

#you can change nfs server and nfs path
helm install nfs-subdir-external-provisioner nfs-subdir-external-provisioner/nfs-subdir-external-provisioner \
    --set nfs.server=10.0.0.10 \
    --set nfs.path=/nfs \
    --set storageClass.defaultClass=true \
    --set storageClass.accessModes=ReadWriteMany
    

# aplly yaml for jenkins 
kubectl apply -f jenkins-pvc.yaml
kubectl apply -f jenkins-deploy.yaml
kubectl apply -f jenkins-service.yaml
kubectl apply -f nexsus-pvc.yaml
kubectl apply -f nexsus-deploy.yaml
kubectl apply -f nexsus-service.yaml

