Ansible VM 1.11
===============

This virtual machine configuration is designed to have ONE application per machine. However, it does support multiple domains / sites per configuration. This virtual machine is a particularly good fit if you run Ubuntu 14.04 LTS on your servers.

- [Ubuntu](http://www.ubuntu.com/) 14.04 Trusty 64bit
- [NGINX](http://nginx.org/) + [PHP5-FPM](http://php-fpm.org/) _(optional)_
- [PHP](http://php.net/) 5.6 _(optional)_
- [NodeJS](http://nodejs.org/) v0.10.29 _(optional)_
- [MailCatcher](http://mailcatcher.me/) _(optional)_
- [Beanstalkd](http://kr.github.io/beanstalkd/) _(optional)_
- [Redis](http://redis.io) _(optional)_
- [R](http://r-project.org) _(optional)_
- [Java](http://java.com) _(optional)_
- [Typesafe Activator](http://scala-lang.org) _(optional)_
- [EventStore](http://geteventstore.com) _(optional)_
- Site configuration defined in the [Vagrantfile](https://github.com/heybigname/ansible/blob/master/Vagrantfile)
- Database configuration defined in the [Vagrantfile](https://github.com/heybigname/ansible/blob/master/Vagrantfile)
- Custom PHP.ini configurations can be defined in the [Vagrantfile](https://github.com/heybigname/ansible/blob/master/Vagrantfile)
- Arbitrary Ruby Gems can be installed from the [Vagrantfile](https://github.com/heybigname/ansible/blob/master/Vagrantfile)

# Usage

1. Bring this repo into your project as the ansible/ folder in the root. You could just copy it there or add it as a submodule: `git submodule add git@github.com:heybigname/ansible.git`
2. Copy the [Vagrantfile](https://github.com/heybigname/ansible/blob/master/Vagrantfile) into your application root
3. Modify your [Vagrantfile](https://github.com/heybigname/ansible/blob/master/Vagrantfile)
    - set the name for the box (the part that says CHANGE ME)
    - configure your hostname, database details, site details, etc
4. Install [Vagrant](http://vagrantup.com), [VirtualBox](https://www.virtualbox.org/), and [Ansible](http://www.ansible.com/home).
5. Just run `vagrant up`

# List of Options

Here is a list of options and their default values.

```ruby
hostname: "dev"
dbuser: "root"
dbpasswd: "password"
databases: []
sites: []
install_db: "no"
install_web: "no"
install_ohmyzsh: "no"
install_hhvm: "no"
install_mailcatcher: "no"
install_beanstalkd: "no"
install_redis: "no"
install_javascript_build_system: "no"
install_gems: []
install_r: "no"
r_packages: []
install_java: "no"
install_typesafe_activator: "no"
typesafe_activator_version: "1.2.10"
install_eventstore: "no",
eventstore_version: "3.0.1",
enable_swap: "no",
swap_size_in_mb: "1024"
```

# Example Vagrantfiles

**PHP / NGINX**

```ruby
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
            php_configs: ["upload_max_filesize=100M", "post_max_size=100M"],
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
            enable_swap: "no",
            swap_size_in_mb: "1024",
            r_packages: []
        }
    end
end
```

**Scala**

```ruby
Vagrant.configure("2") do |config|
    config.vm.box = "trusty64"
    config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"

    config.vm.network :private_network, ip: "10.10.10.10"

    config.vm.provider :virtualbox do |v|
        v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        v.customize ["modifyvm", :id, "--memory", 1024]
        v.customize ["modifyvm", :id, "--name", "CHANGE ME BEFORE USE"]
    end

    config.vm.provision "ansible" do |ansible|
        ansible.playbook = "ansible/provision.yml"
        ansible.extra_vars = {
            install_typesafe_activator: "yes"
        }
    end
end
```

**EventStore**

```ruby
Vagrant.configure("2") do |config|
    config.vm.box = "trusty64"
    config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"

    config.vm.network :private_network, ip: "10.10.10.10"

    config.vm.provider :virtualbox do |v|
        v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        v.customize ["modifyvm", :id, "--memory", 1024]
        v.customize ["modifyvm", :id, "--name", "CHANGE ME BEFORE USE"]
    end

    config.vm.provision "ansible" do |ansible|
        ansible.playbook = "ansible/provision.yml"
        ansible.extra_vars = {
            bind_ip: "10.10.10.10",
            install_eventstore: "yes",
            install_eventstore_version: "3.0.1"
        }
    end
end
```

Changelog
=========

**1.12**

Added optional swap creation

**1.11**

Change Scala to Typesafe Activator
Add EventStore 

**1.10**

Updated PHP from 5.5 to 5.6

**1.9**

Change implementation of PHP configurations

**1.8**

Add Java / Scala Activator support.

**1.7**

Allow installation of R and R packages. Created default values for every option, so no more errors when you upgrade.

**1.6**

Allow installation of Gems and custom PHP.ini declarations from Vagrantfile.

**1.5**

Add redis as an optional package

**1.4**

Add beanstalkd as an optional package
change zsh theme

**1.3**

Add Mailcatcher to Upstart

**1.2**

Added oh-my-zsh configuration (got tired of bash...)
