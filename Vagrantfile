# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # Disable folder syncing SMB1 is disabled by default on Windows 10
  config.vm.synced_folder ".", "/vagrant", disabled: true  
  config.vm.box = "ubuntu/bionic64"
  config.vm.provider "hyperv" do |v, override|
    override.vm.box = "hashicorp/bionic64"
  end
  config.vm.provision "shell", path: "provision/provision.sh"

  # Disable vbguest autoupdate
  if Vagrant.has_plugin?("vagrant-vbguest") then
    config.vbguest.auto_update = false
  end

  config.vm.define "master" do |master|
    master.vm.hostname = "master"
    master.vm.network "private_network", ip: "192.168.33.20"
    master.vm.provision "shell", path: "provision/master_init.sh", privileged: false
    for p in [:virtualbox, :libvirt, :hyperv] do
      master.vm.provider p do |provider|
        provider.memory = 2048
        provider.cpus = 2
      end
    end
  end

  (1..2).each do |i|
    config.vm.define "node#{i}" do |node|
      node.vm.hostname = "node#{i}"
      node.vm.network "private_network", ip: "192.168.33.2#{i}"
      for p in [:virtualbox, :libvirt, :hyperv] do
        node.vm.provider p do |provider|
          provider.memory = 1024
          provider.maxmemory = 2048
          provider.cpus = 2
        end
      end
    end
  end
end