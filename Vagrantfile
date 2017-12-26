#
# Hints and ideas:
#  https://raw.githubusercontent.com/patrickdlee/vagrant-examples/master/example6/Vagrantfile
# 
nodes = [
  { :hostname => 'gw1', :ip => '192.168.70.1', :box => 'centos/7', :ext_nic => "True" },
  { :hostname => 'dns1', :ip => '192.168.70.5', :box => 'centos/7' },
  { :hostname => 'lb1', :ip => '192.168.70.10', :box => 'centos/7' },
  { :hostname => 's1', :ip => '192.168.71.64', :box => 'centos/7' },
  { :hostname => 's2', :ip => '192.168.71.65', :box => 'centos/7' },
  { :hostname => 's3', :ip => '192.168.71.66', :box => 'centos/7' },
  { :hostname => 's4', :ip => '192.168.71.67', :box => 'centos/7' },
]

Vagrant.configure("2") do |config|
  nodes.each do |node|
    config.vm.define node[:hostname] do |nodeconfig|
      nodeconfig.vm.box = node[:box]
      nodeconfig.vm.hostname = node[:hostname] + ".box"
      nodeconfig.vm.network :private_network, ip: node[:ip]
      memory = node[:ram] ? node[:ram] : 256
      nodeconfig.vm.provider :virtualbox do |vb|
        vb.customize [
          "modifyvm", :id,
          "--cpuexecutioncap", "50",
          "--memory", memory.to_s,
        ]
      end
      # If ext_nic defined, setup a connection to the public_network
    end
  end
end

