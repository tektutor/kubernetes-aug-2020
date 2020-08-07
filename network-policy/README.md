In order to try this lab exerices, you need to have a working kubernetes cluster.

It is important that the kubeadm init was done as shown below while bootstrapping master node.
sudo kubeadm init --pod-network-cidr=192.168.0.0/16

The assumption is that you are in network-policy folder.

First you may deploy nginx as shown below

<b>Terminal  1<b>
kubectl apply -f nginx-deploy.yml
kubectl apply -f nginx-service.yml

<b>Terminal 2</b>
You may now try this command from a separate terminal
kubectl run -i --tty busybox -n policy-demo --image=yauritux/busybox-curl --restart=Never -- sh

<b>Terminal 3<b>
From another terminal, you may try this
kubectl run -i --tty bb -n policy-demo --image=yauritux/busybox-curl --restart=Never -- sh


<b>From the Terminal 1</b>
Now you may verify if nginx pods and two busybox pods are running under policy-demo namespace
kubectl get pods -n policy-demo

<b>From Terminal 2 and 3</b>
You can try to access http://nginx
This time, you should be able to access the nginx nodeport service as network policy isn't applied yet.

<b>From Terminal 1<b>
kubectl apply -f deny-all.yml

<b>From Terminal 2 and 3</b>
From busybox and bb pods, you should not be able to access http://nginx

<b>Move back to Terminal 1</b>
kubectl apply -f access-only-for-restricted-pods.yml

<b>From  Terminal 2 (busybox pod)</b>
http://nginx should be possible

<b>From Terminal 3 (bb pod)</b>
http://nginx should not work.

