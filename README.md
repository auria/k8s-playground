# k8s-playground

This repo requires `vagrant` and `virtualbox` tools to be available, as
well as `lxdbr0` bridge to be configured on the host.

Vagrant spec configures a couple of CentOS 7 VMs (4GB RAM, 4 vcpus) with
the default settings to run Kubernetes master and node services
(kubelet, docker, mainly).

A couple of files are copied for SELinux (remove default
enforcement) and sysctl purposes (netfilter parameters), and several
commands are run once (the first time).

Useful vagrant commands:
```
vagrant up
vagrant status
vagrant ssh master01
vagrant ssh node01
vagrant destroy
```

To build a Kubernetes cluster:
* master01: initialize the cluster and configure a SDN (ie. calico)
```
kubeadm init --pod-network-cidr=192.168.200.0/23
kubectl apply -f https://docs.projectcalico.org/v2.6/getting-started/kubernetes/installation/hosted/kubeadm/1.6/calico.yaml
```

* node01: copy the `kubeadm join` line from the master node

Note: swap is disabled manually but reboots will reconfigure it, which
will avoid kubelet from starting up.
