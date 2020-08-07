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



