# coding: utf-8
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  #  config.vm.box = "rhel-cdk-kubernetes-7.3"
  config.vm.box = "centos/7"

  config.vm.provider "virtualbox" do |vb|
    #vb.gui = true    
    vb.cpus = "2"
    vb.memory = "2500"
  end

  
  config.ssh.forward_x11 = true
  
  #  config.vbguest.auto_update = false 
  config.vm.synced_folder ".", "/vagrant", type: "virtualbox"

  config.vm.provision "shell", path: "bin/provision"

  config.vm.define "default", primary: true do |c|
    c.vm.hostname = "jon-server"
    c.vm.network "private_network", ip: "192.168.50.2"
  end
    
  
  config.vm.define "jboss" do |c|
    c.vm.hostname = "jboss"
    c.vm.network "private_network", ip: "192.168.50.3"
    c.vm.provider "virtualbox" do |vb|
      vb.cpus = "1"
      vb.memory = "512"
    end
  end

  
end
