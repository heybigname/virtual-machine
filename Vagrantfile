Vagrant.configure("2") do |config|
	config.vm.box = "precise64"
	config.vm.box_url = "http://files.vagrantup.com/precise64.box"

	config.vm.network :private_network, ip: "10.10.10.10"

	config.vm.provider :virtualbox do |v|
		v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
		v.customize ["modifyvm", :id, "--memory", 512]
		v.customize ["modifyvm", :id, "--name", "occur-vm"]
	end

	config.vm.synced_folder "./", "/var/www/app", id: "vagrant-root", :owner => "www-data"

	config.vm.provision "ansible" do |ansible|
		ansible.playbook = "ansible/provision.yml"
        ansible.extra_vars = {
            databases: ['development']
        }
	end
end
