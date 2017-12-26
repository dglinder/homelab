# -*- mode: ruby -*-
# vi: set ft=ruby :
#
# Usage hints:
#  vagrant destroy && vi Vagrantfile
#  vagrant validate && sudo systemctl start firewalld && vagrant up && sudo systemctl stop firewalld
#  vagrant validate && vagrant destroy && sudo systemctl start firewalld && vagrant up && sudo systemctl stop firewalld
#
# Hints and ideas:
#  https://raw.githubusercontent.com/patrickdlee/vagrant-examples/master/example6/Vagrantfile
# Setup libvirt with remote ssh accesss
#   http://wiki.libvirt.org/page/SSHPolicyKitSetup
#   https://github.com/vagrant-libvirt/vagrant-libvirt
# 
nodes = [
  { :hostname => 'gw1', :ip => '192.168.70.2', :netmask => "255.255.255.0", :box => 'centos/7', :ext_port => 2002 },
  { :hostname => 'dns1', :ip => '192.168.70.5', :netmask => "255.255.255.0", :box => 'centos/7', :ext_port => 2005 },
  { :hostname => 'lb1', :ip => '192.168.70.10', :netmask => "255.255.255.0", :box => 'centos/7', :ext_port => 2010 },
  { :hostname => 's1', :ip => '192.168.71.64', :netmask => "255.255.255.0", :box => 'centos/7', :ext_port => 2064 },
#  { :hostname => 's2', :ip => '192.168.71.65', :netmask => "255.255.255.0", :box => 'centos/7', :ext_port => 2065 },
#  { :hostname => 's3', :ip => '192.168.71.66', :netmask => "255.255.255.0", :box => 'centos/7', :ext_port => 2066 },
#  { :hostname => 's4', :ip => '192.168.71.67', :netmask => "255.255.255.0", :box => 'centos/7', :ext_port => 2067 },
]

Vagrant.configure("2") do |config|
  nodes.each do |node|
    config.vm.define node[:hostname] do |nodeconfig|
      nodeconfig.vm.box = node[:box]
      nodeconfig.vm.hostname = node[:hostname] + ".box"
      nodeconfig.vm.network :private_network, adapter: 1, ip: node[:ip]
      nodeconfig.vm.synced_folder "./" + node[:hostname], '/vagrant', type: 'rsync'
      nodeconfig.vm.network :forwarded_port, guest: 22, host: node[:ext_port], host_ip: "*", gateway_ports: true
      memory = node[:ram] ? node[:ram] : 256
      nodeconfig.vm.provider :virtualbox do |vb|
        vb.customize [
          "modifyvm", :id,
          "--cpuexecutioncap", "50",
          "--memory", memory.to_s,
        ]
      end
      nodeconfig.vm.provision :ansible do |ansible_common|
        ansible_common.playbook = "./common/playbook.yml"
      end
      nodeconfig.vm.provision :ansible do |ansible_host|
        ansible_host.playbook = "./" + node[:hostname] + "/playbook.yml"
      end
    end
  end
end

