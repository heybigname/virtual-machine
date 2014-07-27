Ansible VM
==========

- Ubuntu 14.04 Trusty 64bit
- NGINX + PHP5-FPM
- PHP 5.5
- NodeJS v0.10.29
- Automatic NGINX Site Configuration
- Automatic Database Configuration

# Usage

1. Copy the Vagrantfile into your application root
2. Modify your Vagrantfile
    - set the name for the box (the part that says CHANGE ME)
    - configure your hostname, database details, site details, etc
3. Install Vagrant, VirtualBox, and Ansible.
4. Just run `vagrant up`
