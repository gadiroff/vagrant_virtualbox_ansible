# -*- mode: ruby -*-
# vi: set ft=ruby :
VAGRANTFILE_API_VERSION = "2"
Vagrant.require_version ">= 1.5.0"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.hostmanager.enabled = true
  config.hostmanager.manage_guest = true
  config.hostmanager.manage_host = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true

  config.vm.define "server-01" do |vmconfig|
    config.vm.network "private_network", ip: "172.28.128.101"
    vmconfig.vm.hostname = "server-01"
    vmconfig.vm.box = "ubuntu/disco64"
    vmconfig.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "2048"]
      vb.customize ["modifyvm", :id, "--cpus", "2"]
      vb.customize ["modifyvm", :id, "--ioapic", "on"]
      vb.customize ["modifyvm", :id, "--cpuexecutioncap", "100"]
      vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
      vb.customize ["modifyvm", :id, "--paravirtprovider", "default"]
    end
    vmconfig.vm.provider "parallels" do |v, override|
      override.vm.box = "parallels/centos-6.7"
    end
    vmconfig.vm.provider "aws" do |v, override|
      override.vm.box = "server-01"
    end
    vmconfig.hostmanager.aliases = %w(server-01.local)
    vmconfig.vm.network "forwarded_port", guest: 80, host: 8080
    config.vm.provision "ansible_local" do |ansible|
      ansible.install_mode = "pip"
      ansible.verbose = "v"
      ansible.playbook = "ansible/playbook.yml"
      ansible.raw_arguments = ["--connection=paramiko"]
    end
  end

  config.vm.define "server-02" do |vmconfig|
    config.vm.network "private_network", ip: "192.168.50.4"
    vmconfig.vm.hostname = "server-02"
    vmconfig.vm.box = "ubuntu/disco64"
    vmconfig.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "2048"]
      vb.customize ["modifyvm", :id, "--cpus", "2"]
      vb.customize ["modifyvm", :id, "--ioapic", "on"]
      vb.customize ["modifyvm", :id, "--cpuexecutioncap", "100"]
      vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
      vb.customize ["modifyvm", :id, "--paravirtprovider", "default"]
    end
    vmconfig.vm.provider "parallels" do |v, override|
      override.vm.box = "parallels/centos-6.7"
    end
    vmconfig.vm.provider "aws" do |v, override|
      override.vm.box = "server-02"
    end
    vmconfig.hostmanager.aliases = %w(server-02)
    
    vmconfig.vm.network "forwarded_port", guest: 80, host: 8016
    config.vm.provision "ansible_local" do |ansible|
      ansible.install_mode = "pip"
      ansible.verbose = "v"
      ansible.playbook = "ansible/playbook.yml"
      ansible.raw_arguments = ["--connection=paramiko"]
    end
  end

end
