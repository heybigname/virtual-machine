PHP Development Virtual Machine 5.0
===============

This virtual machine configuration is designed to have ONE application per machine. However, it does support multiple domains / sites per configuration. This virtual machine is a particularly good fit if you run Ubuntu on your servers.

Ansible does not run on Windows but this configuration installs and runs it within the client machine instead of the Windows host so it is now Windows compatible.

- [Ubuntu](http://www.ubuntu.com/) _(tested 16.04 and 18.04, configurable)_ 
- [NGINX](http://nginx.org/) + [PHP7-FPM](http://php-fpm.org/) _(optional)_
- [PHP](http://php.net/) _(optional)_
- [NodeJS](http://nodejs.org/) _(optional)_
- [Beanstalkd](http://kr.github.io/beanstalkd/) _(optional)_
- [Redis](http://redis.io) _(optional)_
- [MySQL](https://mysql.com) _(optional)_
- [Postgresql](https://www.postgresql.org/) _(optional)_
- [RabbitMQ](https://www.rabbitmq.com/) _(optional)_
- [Supervisord](http://supervisord.org/)
- [Smenu](https://github.com/p-gen/smenu)

- site configuration defined in [vm_config.json](https://github.com/heybigname/virtual-machine/blob/master/vm_config.json)
- custom NGINX site configuration is optional
- database configuration defined in [vm_config.json](https://github.com/heybigname/virtual-machine/blob/master/vm_config.json)
- custom PHP.ini configurations can be defined in [vm_config.json](https://github.com/heybigname/virtual-machine/blob/master/vm_config.json)
- arbitrary Ruby Gems can be installed from [vm_config.json](https://github.com/heybigname/virtual-machine/blob/master/vm_config.json)

# Usage

1. Bring this repo into your project as the virtual-machine/ (optional) folder in the root. You could just copy it there or add it as a submodule: `git submodule add git@github.com:heybigname/virtual-machine.git`
2. Copy the [Vagrantfile](https://github.com/heybigname/virtual-machine/blob/master/Vagrantfile) into your repository root folder and modify it.
    - set the name for the box (the part that says CHANGE ME)
    - change Vagrantfile SSH key synced folders line so that ~/.ssh is your host's ssh folder
3. Copy the [vm_config.json](https://github.com/heybigname/virtual-machine/blob/master/vm_config.json) file into your repository root folder and modify it.
    - configure your hostname, database details, site details, etc
4. Install [Vagrant](http://vagrantup.com) and [VirtualBox](https://www.virtualbox.org/).
5. Just run `vagrant up`

# List of Options

Here is a list of options and their default values.

```yaml
hostname: "dev"
dbuser: "root"
dbpasswd: "password"
databases: []
sites: []
php_version: "7.4"
php_modules: ['php{{ php_version }}-mysql', 'php{{ php_version }}-gd', 'php-apcu', 'php{{ php_version }}-curl', 'php{{ php_version }}-intl', 'php-memcached', 'php{{ php_version }}-mbstring', 'php{{ php_version }}-xml', 'php{{ php_version }}-pgsql', 'php{{ php_version }}-dev']
install_db: "no"
install_nginx: "no"
install_php: "no"
install_beanstalkd: "no"
install_redis: "no"
install_javascript_build_system: "no"
node_major_version: "13"
install_gems: []
php_configs: []
install_postgresql: "no"
postgresql_version: "9.5"
postgresql_user: "root"
postgresql_passwd: "password"
postgresql_databases: []
enable_swap: "yes"
swap_size_in_mb: 1024
install_rabbit_mq: "no"
install_smenu: "no"
```

# Example vm_config.json

```ruby
  "hostname": "dev-vm",
  "dbuser": "root",
  "dbpasswd": "password",
  "databases": [
    "development",
    "testing"
  ],
  "sites": [
    {
      "hostname": "app.local",
      "document_root": "/vagrant/public"
    }
  ],
  "php_configs": [
    {
      "option": "upload_max_filesize",
      "value": "100M"
    },
    {
      "option": "post_max_size",
      "value": "100M"
    }
  ],
  "install_redis": "yes",
  "install_db": "yes",
  "install_nginx": "yes",
  "install_php": "yes",
  "php_version": "7.4",
  "install_javascript_build_system": "yes",
  "node_major_version": "10",
  "enable_swap": "yes",
  "swap_size_in_mb": "1024",
  "install_smenu": "yes"
```

Changelog
=========

**6.0**

- Update names (breaking backward compatibility) so nginx / php are now separated. "install_php" "install_nginx". The goal is to reduce provisioning time for command line systems.

**5.3**

- Add ['smenu'](https://github.com/p-gen/smenu)

**5.0**

- Ubuntu 18.04 default (configurable)
- Repository has been renamed from 'ansible' to 'virtual-machine'
- Ansible now runs within the virtual machine making this setup compatible with Windows.
- Removed a number of features including event store, java, scala, R, hhvm, and mailcatcher.
- Provisioning instructions are handled in the Vagrantfile and options are now handled in the vm_options.json file.
- Upgrade PHP to 7.3 (version is configurable)

**4.0**

Upgrade PHP 7.1 to 7.2.

**3.3**

Fix some small bugs and add rabbitmq support.
