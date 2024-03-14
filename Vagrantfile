# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "generic/ubuntu2204"
  
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
    vb.cpus = "1"
  end

  ssh_pub_key = File.readlines("./id_rsa.pub").first.strip

  config.vm.provision "shell", inline: <<-SHELL
    echo #{ssh_pub_key} >> ~vagrant/.ssh/authorized_keys
    echo #{ssh_pub_key} >> ~root/.ssh/authorized_keys
    sudo sed -i 's/\#PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
    systemctl restart sshd
  SHELL
  
  config.vm.define "front" do |front|
    front.vm.hostname = "nginx"
    front.vm.network "private_network", ip: "192.168.111.11", adapter: 2, netmask: "255.255.255.0", virtualbox__intnet: "mynet"
  end

  config.vm.define "back" do |back|
    back.vm.hostname = "apache"
    back.vm.network "private_network", ip: "192.168.111.12", adapter: 2, netmask: "255.255.255.0", virtualbox__intnet: "mynet"
  end

end
