# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.provider :virtualbox do |vb|
    # 時刻同期
    vb.customize ["guestproperty", "set", :id, "--timesync-threshold", 60000]
    vb.customize ["setextradata", :id, "VBoxInternal/Devices/VMMDev/0/Config/GetHostTimeDisabled", 0]
    vb.customize ["modifyvm", :id, "--paravirtprovider", "kvm"]
    vb.memory = "1024"
  end

  config.vm.define "masterdb" do |node|
    node.vm.box = "ubuntu/trusty64"
    node.vm.hostname = "masterdb"
    node.vm.network :private_network, ip: "192.168.33.180" 
  end

  config.vm.define "slavedb" do |node|
    node.vm.box = "ubuntu/trusty64"
    node.vm.hostname = "slavedb"
    node.vm.network :private_network, ip: "192.168.33.181" 
  end

  config.vm.define "training" do |node|
    node.vm.box = "ubuntu/trusty64"
    node.vm.hostname = "training"
    node.vm.network :private_network, ip: "192.168.33.182" 
  end

  config.vm.provision "shell", inline: <<-__SCRIPT__
     sed -i".bak" -e 's@\/\/archive.ubuntu.com@\/\/ftp.jaist.ac.jp@g' /etc/apt/sources.list
     apt-get -y update
     apt-get -y install sysv-rc-conf
     service chef-client stop && sysv-rc-conf --level 3 chef-client off
     service puppet stop && sysv-rc-conf --level 3 puppet off
  __SCRIPT__

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "mysql.yml"
    #ansible.raw_arguments = ['-vvv']
    ansible.groups = {
      "master"   => ["masterdb"],
      "slave"    => ["slavedb"],
      "training" => ["training"],
      "master:vars"   => {"mode" => "master"},
      "slave:vars"    => {"mode" => "slave"},
      "training:vars" => {"mode" => "training"}
    }
  end
end
