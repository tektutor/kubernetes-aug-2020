In order to try this lab exerices, you need to have a working kubernetes cluster.

It is important that the kubeadm init was done as shown below while bootstrapping master node.
sudo kubeadm init --pod-network-cidr=192.168.0.0/16

The assumption is that you are in network-policy folder.

First you may deploy nginx as shown below

Terminal  1
+++++++++++
kubectl apply -f nginx-deploy.yml
kubectl apply -f nginx-service.yml

Terminal 2
++++++++++
You may now try this command from a separate terminal
kubectl run -i --tty busybox -n policy-demo --image=yauritux/busybox-curl --restart=Never -- sh

Terminal 3
++++++++++
From another terminal, you may try this
kubectl run -i --tty bb -n policy-demo --image=yauritux/busybox-curl --restart=Never -- sh


From the Terminal 1
+++++++++++++++++++
Now you may verify if nginx pods and two busybox pods are running under policy-demo namespace
kubectl get pods -n policy-demo

From Terminal 2 and 3
+++++++++++++++++++++
You can try to access http://nginx
This time, you should be able to access the nginx nodeport service as network policy isn't applied yet.

From Terminal 1
+++++++++++++++
kubectl apply -f deny-all.yml

From Terminal 2 and 3
+++++++++++++++++++++
From busybox and bb pods, you should not be able to access http://nginx

Move back to Terminal 1
+++++++++++++++++++++++
kubectl apply -f access-only-for-restricted-pods.yml

From  Terminal 2 (busybox pod)
++++++++++++++++++++++++++++++
http://nginx should be possible

From Terminal 3 (bb pod)
+++++++++++++++++++++++++
http://nginx should not work.

