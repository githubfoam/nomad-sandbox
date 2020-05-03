# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|

  # config.vbguest.auto_update = false

  1.upto(3) do |i|
    name = "nomad-server#{i}"
    ip = "192.168.33.1#{i}"
    config.vm.define name do |n|
      n.vm.box = "ubuntu/bionic64"
      n.vm.network "private_network", ip: ip
      n.vm.provision "ansible_local" do |ansible|
        ansible.playbook = "site.yml"
        ansible.inventory_path = "inventory"
      end
    end
  end

  1.upto(2) do |i|
    name = "nomad-client#{i}"
    ip = "192.168.33.2#{i}"
    config.vm.define name do |n|
      n.vm.box = "ubuntu/bionic64"
      n.vm.network "private_network", ip: ip
      n.vm.provision "ansible_local" do |ansible|
        ansible.playbook = "site.yml"
        ansible.inventory_path = "inventory"
      end
    end
  end
end
