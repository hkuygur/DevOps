# DevOps

#install vagrant and vagrant vmware tools
#run this command
vagrant up
#after running machines
#run kubernetes-config.sh
./kubernetes-config.sh
#Then, in master node:
kubeadm init
#this gives you a command for worker nodes join the master node
#check the node status
kubectl get nodes
# aplly yaml for jenkins 
kubectl apply -f jenkins-pvc.yaml
kubectl apply -f jenkins-deploy.yaml
kubectl apply -f jenkins-service.yaml
kubectl apply -f nexsus-pvc.yaml
kubectl apply -f nexsus-deploy.yaml
kubectl apply -f nexsus-service.yaml

