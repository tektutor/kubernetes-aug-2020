# kubernetes-aug-2020

You will need a Kubernetes Cluster to try these lab exercises.

## Instructions to install Docker in Ubuntu

sudo apt-get update
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
    
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs)  stable"
   
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io


## On CentOS machines

sudo yum install -y yum-utils

sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
    
sudo yum install docker-ce docker-ce-cli containerd.io

You need to enable and start the docker service<br>
sudo systemctl enable docker && sudo systemctl start docker

## You may now install kubectl, kubeadm and kubelet 

Step 1 - To ensure the IP Table on Kubernetes Nodes see all the bridged traffic, you need to perform the below configuration.<br>
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sudo sysctl --system

Step 2 
sudo apt-get update && sudo apt-get install -y apt-transport-https curl
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF

sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl

## You may bootstrap master node as shown below
sudo kubeadm init --pod-network-cidr=192.168.0.0/16

You can install calico cni as shown below
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml

You need to untaint master node to schedule user pods on master node
kubectl taint node <your node name> node-role.kubernetes.io/master:NoSchedule-

You are expected to have a working single node K8 cluster<br>
kubectl get nodes
