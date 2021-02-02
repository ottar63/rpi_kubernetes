# RPI_Kubernetes

Was trying to automate installation of kubernetes on my Raspberry PI cluster.

Unfortunatly k8s requires at leas 1700 MB memory to run, so that did not work

Next attempt was to install k3s, lightweigth kubernetes.

And that did work.

My cluster consist of 4 Raspberry PI 3B.

My dhcp server is setup to give these server fixed address from 192.168.1.51 to 192.168.1.54.

Ansible is running on Virtualbox on my PC with  Ubuntu 20.04.

This is how I run the setup:
```
$ansible-playbook -i ./hosts.ini rpi_basic.yml --ask-pass
```
This does some basic configuration of the Raspberry PI's

Next step is install k3s, that is done with:
```
$ansible-playbook -i ./hosts.ini  k3s.yml
```
After this is finished, you should be able to login to the master and check the status:
```
$sudo kubectl get nodes
```
And it should give something like this:
```
NAME          STATUS   ROLES                  AGE   VERSION
kube-node3    Ready    <none>                 24m   v1.20.2+k3s1
kube-node1    Ready    <none>                 24m   v1.20.2+k3s1
kube-node2    Ready    <none>                 24m   v1.20.2+k3s1
kube-master   Ready    control-plane,master   27m   v1.20.2+k3s1
```

Next step is finding something to user the cluster for :-)
