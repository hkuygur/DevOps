Vagrant.configure("2") do |config|
  config.vm.define "master" do |master|
    master.vm.box = "bento/ubuntu-20.04"
    master.vm.hostname = "master-node"
    master.vm.network "private_network", ip: "10.0.0.10"
    master.vm.provider "vmware_workstation" do |v|
      v.memory = 2048
      v.cpus = 2
    end
  end

  (1..2).each do |i|
    config.vm.define "node0#{i}" do |node|
      node.vm.box = "bento/ubuntu-20.04"
      node.vm.hostname = "worker-node0#{i}"
      node.vm.network "private_network", ip: "10.0.0.1#{i}"
      node.vm.provider "vmware_workstation" do |v|
        v.memory = 3072
        v.cpus = 2
      end
    end
  end

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update -y
    echo "10.0.0.10  master-node" >> /etc/hosts
    echo "10.0.0.11  worker-node01" >> /etc/hosts
    echo "10.0.0.12  worker-node02" >> /etc/hosts
  SHELL

  config.vm.provision "shell", inline: <<-SHELL
    # Disable swap to meet Kubernetes requirements
    swapoff -a
    sed -i '/swap/d' /etc/fstab

    # Install container runtime (Containerd in this case)
    apt-get update -y && apt-get install -y containerd
    systemctl enable containerd
    systemctl start containerd

    # Install kubeadm, kubelet, and kubectl
    apt-get update -y && apt-get install -y apt-transport-https curl
    curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
    echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" > /etc/apt/sources.list.d/kubernetes.list
    apt-get update -y
    apt-get install -y kubelet=1.25.0-00 kubeadm=1.25.0-00 kubectl=1.25.0-00
	apt-mark hold kubelet kubeadm kubectl
    apt-get install -y nfs-client
  SHELL
end
