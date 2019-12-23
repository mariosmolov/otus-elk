# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "centos/7"

  config.vm.provider "virtualbox" do |v|
      v.memory = 1024
      v.cpus = 1
  end
  config.vm.provision "shell", inline: "mkdir /root/.ssh/ && cat /vagrant/ansible.pub >> /root/.ssh/authorized_keys && cat /vagrant/ansible.pub >> /home/vagrant/.ssh/authorized_keys"
  config.ssh.username = "vagrant"

  config.vm.define "logweb" do |logweb|
    logweb.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "2048"]
    end
    logweb.vm.network "private_network", ip: '192.168.168.110', adapter: 2, netmask: "255.255.255.192", virtualbox__intnet: "sw01"
    logweb.vm.network :forwarded_port, guest: 22, host: 2202, id: "ssh"
    logweb.vm.hostname = "logweb"
  end

  config.vm.define "web01" do |web01|
    web01.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "256"]
    end
    web01.vm.network "private_network", ip: '192.168.168.100', adapter: 2, netmask: "255.255.255.192", virtualbox__intnet: "sw01"
    web01.vm.network :forwarded_port, guest: 22, host: 2203, id: "ssh"
    web01.vm.hostname = "web01"
  end

  config.vm.provision "ansible" do |ansible|
    ansible.verbose = "v"
    ansible.playbook = "provision/playbook.yml"
  end

end