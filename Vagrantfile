# -*- mode: ruby -*-
# vi: set ft=ruby :

$script = <<-SCRIPT
echo I am provisioning...

cd /vagrant/setup
sudo ./server_init_harden.sh
sudo cp -rvf /vagrant /home/vagrant/writerviet
cd /home/vagrant/writerviet

# # initialize elasticsearch config volume
# cd ~/writerviet/elasticsearch/config
# docker run --mount type=volume,source=writerviet_elasticsearch_config,target=/data --name helper alpine
# sudo docker cp . helper:/data
# docker rm helper

# # initialize kibana config volume
# cd ~/writerviet/kibana/config
# docker run --mount type=volume,source=writerviet_kibana_config,target=/data --name helper alpine
# sudo docker cp . helper:/data
# docker rm helper

SCRIPT

Vagrant.configure("2") do |config|
  # Base VM OS configuration.
  config.vm.box = "ubuntu/bionic64"

  # General VirtualBox VM configuration.
  config.vm.provider :virtualbox do |v|
    v.name = "writerviet-monitoring-stack"
    v.memory = 2048 # atleast 2G for elasticsearch
    v.cpus = 2
    v.linked_clone = true
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize ["modifyvm", :id, "--ioapic", "on"]
  end

  #
  # ssh vagrant box as admin user:
	#   ssh -p 2222 admin@localhost
	#   ssh admin@192.168.2.2
	#
  # to provision ansible playbook
  #    vagrant provision
  #
	config.vm.network :private_network, ip: "192.168.2.3"
  config.vm.provision "shell", inline: $script
end
