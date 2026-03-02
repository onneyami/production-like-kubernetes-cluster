# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"
  config.vm.box_check_update = false
  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.provider "virtualbox" do |vb|
    vb.memory = 2048
    vb.cpus = 2
  end

  # Control Plane Node
  config.vm.define "control-plane" do |node|
    node.vm.hostname = "control-plane"
    node.vm.network "private_network", ip: "192.168.56.10"
    node.vm.provider "virtualbox" do |vb|
      vb.name = "k8s-control-plane"
    end
  end

  # Worker Nodes
  (1..2).each do |i|
    config.vm.define "worker-#{i}" do |node|
      node.vm.hostname = "worker-#{i}"
      node.vm.network "private_network", ip: "192.168.56.#{10 + i}"
      node.vm.provider "virtualbox" do |vb|
        vb.name = "k8s-worker-#{i}"
      end
    end
  end

  # Installing Python (for Ansible)
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y python3 python3-apt
  SHELL
end