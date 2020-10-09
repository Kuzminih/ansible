# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "centos/7"

  config.vm.provider "virtualbox" do |v|
    v.memory = 256
    v.cpus = 1
  end

  config.vm.define "ansible" do |ansible|
    ansible.vm.network "private_network", ip: "192.168.50.10", virtualbox__intnet: "net1"
    ansible.vm.hostname = "ansible"
    ansible.vm.provision "shell", inline: <<-SHELL
      mkdir -p ~root/.ssh; cp ~vagrant/.ssh/auth* ~root/.ssh
    SHELL
    ansible.vm.provision "shell", path: "ansible.sh"
  end

  config.vm.define "nginx" do |nginx|
    nginx.vm.network "private_network", ip: "192.168.50.11", virtualbox__intnet: "net1"
    nginx.vm.hostname = "nginx"
    nginx.vm.provision "shell", path: "nginx.sh"
  end

end
