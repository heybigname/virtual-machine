# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
    config.vm.box = "ubuntu/bionic64"
    config.vm.network "private_network", ip: "10.10.10.10"
    config.vm.box_check_update = false
    config.vm.hostname = "vm"

    config.vm.provider "virtualbox" do |v|
       v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
       v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
       v.customize ["modifyvm", :id, "--memory", "1024"]
       v.customize ["modifyvm", :id, "--name", "CHANGE ME BEFORE USAGE"]
    end

    # set up ssh for inside-machine ansible. Change ~/.ssh to your host's ssh keys path.
    config.vm.synced_folder ".", "/vagrant"
    config.vm.synced_folder "~/.ssh", "/home/vagrant/ssh-host"

    # install ansible inside the machine then provision with it
    # provisioning configuration is vm_config.json
    config.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update -y
        sudo apt-get install -y software-properties-common
        sudo apt-get install -y python
        sudo apt-add-repository ppa:ansible/ansible
        sudo apt-get update -y
        sudo apt-get install -y ansible
        cp /home/vagrant/ssh-host/* /home/vagrant/.ssh/ && chown -R vagrant /home/vagrant/.ssh
        ansible-playbook -i /vagrant/virtual-machine/hosts.ini /vagrant/virtual-machine/provision.yml --extra-vars="@/vagrant/vm_config.json" && exit 0
    SHELL

    # perform any additional provisioning here
    config.vm.provision "shell", inline: <<-SHELL
        cd /vagrant
    SHELL
end
