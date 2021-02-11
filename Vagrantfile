# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
    config.vm.box = "geerlingguy/ubuntu1804"
    config.vm.box_version = "1.0.0"
    config.vm.box_check_update = false
    
    config.vm.network "private_network", ip: "10.10.10.10"
    
    config.vm.hostname = "vm"

    # default to false because security isn't important
    # and some people may have trouble with their SSH environment.
    config.ssh.insert_key = false
    
    config.vm.provider "virtualbox" do |v|
        v.cpus = 2
        v.memory = 2048
        v.customize ["modifyvm", :id, "--audio", "none"]
        v.customize ["modifyvm", :id, "--vram", "16"]
        v.customize ["modifyvm", :id, "--graphicscontroller", "none"]
        v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
        v.customize ["modifyvm", :id, "--name", "my-vm"]
        # options
        # v.customize ["modifyvm", :id, "--paravirtprovider", "default"]
        # v.customize ["modifyvm", :id, "--pae", "on"]
        # v.customize ["modifyvm", :id, "--largepages", "on"]        
        # v.customize ["storagectl", :id, "--name", "IDE Controller", "--hostiocache", "on"]
    end

    # timezone
    config.timezone.value = "Europe/Amsterdam"
    
    # set up ssh for inside-machine ansible. Change ~/.ssh to your host's ssh keys path.
    config.vm.synced_folder ".", "/vagrant"
    config.vm.synced_folder "~/.ssh", "/home/vagrant/ssh-host"
    
    config.vm.provision "file", source: "~/.gitconfig", destination: ".gitconfig"

    # install ansible inside the machine then provision with it
    # provisioning configuration is vm_config.json
    config.vm.provision "shell", inline: <<-SHELL
        export DEBIAN_FRONTEND=noninteractive
        export ANSIBLE_ACTION_WARNINGS=false
        export ANSIBLE_STRING_CONVERSION_ACTION=ignore
        
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
