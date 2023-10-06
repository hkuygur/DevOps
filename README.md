## Dependencies

You should install [VirtualBox](https://www.virtualbox.org/wiki/Downloads) or 
[VMWare Workstation](https://www.vmware.com/products/workstation-pro/workstation-pro-evaluation.html)[Vagrant](https://www.vagrantup.com/downloads.html) 
and [Vagrant](https://www.vagrantup.com/downloads.html) before you start.
In this document, the installations are performed on VMWare Workstation.
If you want to install on vmware, you need to install [Vmware utility tool](https://developer.hashicorp.com/vagrant/docs/providers/vmware/vagrant-vmware-utility).
Also you should install vagrant-vagrant-vmware-desktop

```bash
$ vagrant plugin install vagrant-vmware-desktop
```


Copy the Vagrantfile and run this command also you can modify Vagrantfile 

```bash
vagrant up
```

after running machines
run this command

```bash
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

```

Then, in master node:

```bash
kubeadm init
```
this gives you a command for worker nodes join the master node
copy the command and run each worker node
check the node status

```bash
kubectl get nodes
```
helm installing

```bash
curl -k https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```
calico

```bash
curl https://docs.projectcalico.org/manifests/calico.yaml -O
kubectl apply -f calico.yaml
```

metric

```bash
curl -k https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml -O
kubectl apply -f components.yaml
```
nfs-subdir

```bash
helm repo add nfs-subdir-external-provisioner https://kubernetes-sigs.github.io/nfs-subdir-external-provisioner/ --insecure-skip-tls-verify
```

you can change nfs server and nfs path

```bash
helm install nfs-subdir-external-provisioner nfs-subdir-external-provisioner/nfs-subdir-external-provisioner \
    --set nfs.server=10.0.0.10 \
    --set nfs.path=/nfs \
    --set storageClass.defaultClass=true \
    --set storageClass.accessModes=ReadWriteMany
    
```


aplly yaml for jenkins

```bash
kubectl apply -f jenkins-pvc.yaml
kubectl apply -f jenkins-deploy.yaml
kubectl apply -f jenkins-service.yaml
kubectl apply -f nexsus-pvc.yaml
kubectl apply -f nexsus-deploy.yaml
kubectl apply -f nexsus-service.yaml

```
