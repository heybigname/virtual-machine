Ansible VM 1.1
==============

- [Ubuntu](http://www.ubuntu.com/) 14.04 Trusty 64bit
- [NGINX](http://nginx.org/) + [PHP5-FPM](http://php-fpm.org/) _(optional)_
- [PHP](http://php.net/) 5.5 _(optional)_
- [NodeJS](http://nodejs.org/) v0.10.29 _(optional)_
- [MailCatcher](http://mailcatcher.me/) _(optional)_
- [HHVM](http://hhvm.com/) _(optional)_
- Site configuration defined in the [Vagrantfile](https://github.com/heybigname/ansible/blob/master/Vagrantfile)
- Database configuration defined in the [Vagrantfile](https://github.com/heybigname/ansible/blob/master/Vagrantfile)

# Usage

1. Bring this repo into your project as the ansible/ folder in the root. You could just copy it there or add it as a submodule: `git submodule add git@github.com:heybigname/ansible.git`
2. Copy the [Vagrantfile](https://github.com/heybigname/ansible/blob/master/Vagrantfile) into your application root
3. Modify your [Vagrantfile](https://github.com/heybigname/ansible/blob/master/Vagrantfile)
    - set the name for the box (the part that says CHANGE ME)
    - configure your hostname, database details, site details, etc
4. Install [Vagrant](http://vagrantup.com), [VirtualBox](https://www.virtualbox.org/), and [Ansible](http://www.ansible.com/home).
5. Just run `vagrant up`
