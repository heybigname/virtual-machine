# vagrant init ubuntu/trusty64

Vagrant.configure("2") do |config|
    config.vm.box = "trusty64"
    config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"

    config.vm.network :private_network, ip: "10.10.10.10"

    config.vm.provider :virtualbox do |v|
        v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        v.customize ["modifyvm", :id, "--memory", 512]
        v.customize ["modifyvm", :id, "--name", "dev"]
    end

    config.vm.provision "ansible" do |ansible|
        ansible.playbook = "ansible/provision.yml"
        ansible.extra_vars = {
            hostname: "dev",
            dbuser: "root",
            dbpasswd: "password",
            databases: ["development"],
            sites: [
                {
                    hostname: "app.local",
                    document_root: "/vagrant/public"
                }, {
                    hostname: "app2.local",
                    document_root: "/vagrant/public2"
                }
            ],
            install_javascript_build_system: "no"
        }
    end
end
