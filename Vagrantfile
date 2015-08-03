# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

require 'yaml'
yaml_config = YAML.load_file('vagrant/config.yaml')
hosts = yaml_config['hosts']

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "geerlingguy/centos6"
  config.ssh.insert_key = false
  hosts.each do |name,cfg|
    config.vm.define name do |vm_config|
      vm_config.vm.hostname =  name
      vm_config.vm.network :private_network, ip: cfg['ip']
      vm_config.vm.provider :virtualbox do |vbox|
        vbox.name = name
        vbox.memory = cfg['memory']
      end
    end
    config.vm.provision "ansible" do |ansible|
      ansible.playbook = "repos/ansible-playbook/playbook.yml"
      ansible.groups = {
        "supercool_app_servers" => ["supercool-app-test1", "supercool-app-test2"],
        "haproxy"               => ["haproxy-test1", "haproxy-test2"]
      }
      ansible.extra_vars = {
        haproxy_vip: '192.168.30.100',
        supercool_app_loglevel: 'DEBUG'
      }
    end
  end
end
