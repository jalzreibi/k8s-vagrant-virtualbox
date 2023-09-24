# k8s-vagrant-virtualbox
deploying k8 on virtualbox using vagrant


Vagrant version: Vagrant 2.3.7
Virtualbox version: Version 7.0.10 r158379 (Qt5.15.2)


Go into each directory:
1. run Vagrant up --> this will bring the VM up
2. run Vagrant ssh --> this will allow you to ssh into the box
3. run installk8.sh on each node --> this will upgrade the VM and install all the required tools and configuration
4. restart the nodes and run sudo swapoff -a 

On the master node:
1. run sudo kubeadm init --pod-network-cidr=10.10.0.0/16 --apiserver-advertise-address=192.168.50.10 --> this will start the master node
2. once the master node is up, run kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.26.1/manifests/tigera-operator.yaml --> this will install calico network CNI plugin
3. run wget https://raw.githubusercontent.com/projectcalico/calico/v3.26.1/manifests/custom-resources.yaml --> this will download the custom resources file for the calico CNI
4. edit the custom-resources.yaml, change CIDR to 10.0.0.0/16 and then run kubectl apply -f custom-resources.yaml
5. wait until the master node goes into ready state

On the worker nodes:
1. run the kubeadmin join command outputed at the end of the kubeadmin init command it will look something like this
kubeadm join 192.168.50.10:6443 --token squ6en.7dqxhbtsksa226vw \
--discovery-token-ca-cert-hash sha256:a9ea616399415226d7721a970f3627afe9ef0b81b55595b9bf7403a1e98d52a4


Wait for the nodes to go into Ready state.

Congrats! you just installed your k8 cluster!
