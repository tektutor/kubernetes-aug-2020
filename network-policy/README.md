In order to try this lab exerices, you need to have a working kubernetes cluster.

It is important that the kubeadm init was done as shown below while bootstrapping master node.
sudo kubeadm init --pod-network-cidr=192.168.0.0/16

The assumption is that you are in network-policy folder.

First you may deploy nginx as shown below

<b>Terminal  1<b><br>
kubectl apply -f nginx-deploy.yml<br>
kubectl apply -f nginx-service.yml<br>

<b>Terminal 2</b><br>
You may now try this command from a separate terminal<br>
kubectl run -i --tty busybox -n policy-demo --image=yauritux/busybox-curl --restart=Never -- sh<br>

<b>Terminal 3<b><br>
From another terminal, you may try this<br>
kubectl run -i --tty bb -n policy-demo --image=yauritux/busybox-curl --restart=Never -- sh<br>


<b>From the Terminal 1</b><br>
Now you may verify if nginx pods and two busybox pods are running under policy-demo namespace<br>
kubectl get pods -n policy-demo<br>

<b>From Terminal 2 and 3</b><br>
You can try to access http://nginx<br>
This time, you should be able to access the nginx nodeport service as network policy isn't applied yet.<br>

<b>From Terminal 1<b><br>
kubectl apply -f deny-all.yml<br>

<b>From Terminal 2 and 3</b><br>
From busybox and bb pods, you should not be able to access http://nginx<br>

<b>Move back to Terminal 1</b><br>
kubectl apply -f access-only-for-restricted-pods.yml<br>

<b>From  Terminal 2 (busybox pod)</b><br>
http://nginx should be possible<br>

<b>From Terminal 3 (bb pod)</b><br>
http://nginx should not work.

