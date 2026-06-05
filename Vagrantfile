# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
  
  config.vm.define "firewall" do |fw|
    fw.vm.box = "ubuntu/focal64"
    fw.vm.hostname = "firewall"
    fw.vm.network "private_network", virtualbox__intnet: "Admin_Net", ip: "10.0.1.1"
    fw.vm.network "private_network", virtualbox__intnet: "Server_Net", ip: "10.0.2.1"
    fw.vm.provider "virtualbox" do |vb|
      vb.name = "Lab_Firewall"
      vb.memory = "1024"
      vb.cpus = 1
    end
    fw.vm.provision "shell", inline: <<-SHELL
      echo "net.ipv4.ip_forward=1" >> /etc/sysctl.conf
      sysctl -p
    SHELL
  end

  config.vm.define "client" do |client|
    client.vm.box = "ubuntu/focal64"
    client.vm.hostname = "admin-client"
    client.vm.network "private_network", virtualbox__intnet: "Admin_Net", ip: "10.0.1.10"
    client.vm.provider "virtualbox" do |vb|
      vb.name = "Lab_Client"
      vb.memory = "1024"
      vb.cpus = 1
    end
    client.vm.provision "shell", inline: <<-SHELL
      cat > /etc/netplan/99-routes.yaml << 'EOF'
network:
  version: 2
  ethernets:
    enp0s8:
      routes:
        - to: 0.0.0.0/0
          via: 10.0.1.1
        - to: 10.0.2.0/24
          via: 10.0.1.1
EOF
      netplan apply
    SHELL
  end

  config.vm.define "server" do |srv|
    srv.vm.box = "ubuntu/focal64"
    srv.vm.hostname = "hardened-server"
    srv.vm.network "private_network", virtualbox__intnet: "Server_Net", ip: "10.0.2.10"
    srv.vm.provider "virtualbox" do |vb|
      vb.name = "Lab_Server"
      vb.memory = "1024"
      vb.cpus = 1
    end
    srv.vm.provision "shell", inline: <<-SHELL
      cat > /etc/netplan/99-routes.yaml << 'EOF'
network:
  version: 2
  ethernets:
    enp0s8:
      routes:
        - to: 0.0.0.0/0
          via: 10.0.2.1
        - to: 10.0.1.0/24
          via: 10.0.2.1
EOF
      netplan apply
    SHELL
  end
end