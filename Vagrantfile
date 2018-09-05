# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "centos/7"
  config.vm.network "public_network", bridge: "lxdbr0"

  config.vm.define "master01" do |master01|
    master01.vm.hostname = "master01"
    master01.vm.provider "virtualbox" do |vb|
      vb.name = "centos-master01"
      vb.memory = 4096
      vb.cpus = 4
    end
  end

  config.vm.define "node01" do |node01|
    node01.vm.hostname = "node01"
    node01.vm.provider "virtualbox" do |vb|
      vb.name = "centos-node01"
      vb.memory = 4096
      vb.cpus = 4
    end
  end

  config.vm.provision "shell", inline: <<-SHELL
    test -f /etc/yum.repos.d/kubernetes.repo && exit 0
    echo $(ip -o -4 a s eth1 | awk '{print $4}' | cut -d/ -f1; hostname) | sudo tee -a /etc/hosts
    sudo cp /vagrant/kubernetes.repo /etc/yum.repos.d/kubernetes.repo
    sudo cp /vagrant/selinux-config /etc/selinux/config
    sudo ip r d 0/0 dev eth0
    sudo swapoff -a
    sudo modprobe br_netfilter
    echo "net.bridge.bridge-nf-call-iptables = 1" | sudo tee -a /etc/sysctl.d/50-k8s.conf
    echo "net.ipv4.ip_forward = 1" | sudo tee -a /etc/sysctl.d/50-k8s.conf
    sudo sysctl -p /etc/sysctl.d/*
    sudo setenforce 0
    sudo yum install -y docker kubeadm kubelet kubectl
    sudo systemctl enable docker && sudo systemctl start docker
    sudo systemctl enable kubelet && sudo systemctl start kubelet
  SHELL
end
