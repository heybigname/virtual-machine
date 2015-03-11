# vagrant init ubuntu/trusty64

Vagrant.configure("2") do |config|
    config.vm.box = "trusty64"
    config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"

    config.vm.network :private_network, ip: "10.10.10.10"

    config.vm.provider :virtualbox do |v|
        v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        v.customize ["modifyvm", :id, "--memory", 768]
        v.customize ["modifyvm", :id, "--name", "CHANGE ME BEFORE USE"]
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
                    document_root: "/vagrant/site/public"
                }, {
                    hostname: "app2.local",
                    document_root: "/vagrant/site2/public2"
                }
            ],
            php_configs: [
                { option: "upload_max_filesize", value: "100M" },
                { option: "post_max_size", value: "100M" }
            ],
            install_gems: ["compass", "zurb-foundation"],
            install_db: "yes",
            install_ohmyzsh: "yes",
            install_web: "yes",
            install_mailcatcher: "yes",
            install_hhvm: "yes",
            install_beanstalkd: "no",
            install_redis: "no",
            install_javascript_build_system: "no",
            install_r: "no",
            r_packages: [],
            enable_swap: "no",
            swap_size_in_mb: "1024",
            install_eventstore: "no",
            eventstore_version: "3.0.1",
            r_packages: []
        }
    end
end
