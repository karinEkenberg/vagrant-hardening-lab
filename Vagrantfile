# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  
  # ----------------------------------------------------
  # 1. FIREWALL / ROUTER (Ubuntu Server)
  # ----------------------------------------------------
  config.vm.define "firewall" do |fw|
    fw.vm.box = "ubuntu/focal64"
    fw.vm.hostname = "firewall"
    
    # Adapter 1 is automatically set to NAT (WAN/Internet) by Vagrant.
    # We add two internal networks (isolated switches) for segmentation:
    fw.vm.network "private_network", virtualbox__intnet: "Admin_Net", ip: "10.0.1.1"
    fw.vm.network "private_network", virtualbox__intnet: "Server_Net", ip: "10.0.2.1"
    
    fw.vm.provider "virtualbox" do |vb|
      vb.name = "Lab_Firewall"
      vb.memory = "1024"
      vb.cpus = 1
    end

    # Enable IP forwarding so this VM can act as a router/gateway
    fw.vm.provision "shell", inline: <<-SHELL
      echo "net.ipv4.ip_forward=1" >> /etc/sysctl.conf
      sysctl -p
    SHELL
  end

  # ----------------------------------------------------
  # 2. ADMINISTRATION CLIENT (Ubuntu Server - CLI)
  # ----------------------------------------------------
  config.vm.define "client" do |client|
    client.vm.box = "ubuntu/focal64"
    client.vm.hostname = "admin-client"
    
    # Connected to Admin_Net. Firewall is .1, Client is assigned .10
    client.vm.network "private_network", virtualbox__intnet: "Admin_Net", ip: "10.0.1.10"
    
    config.vm.provider "virtualbox" do |vb|
      vb.name = "Lab_Client"
      vb.memory = "1024"
      vb.cpus = 1
    end

    # Set the firewall IP (10.0.1.1) as the default gateway for this subnet
    client.vm.provision "shell", inline: <<-SHELL
      ip route del default via 10.0.2.2 2> /dev/null || true
      ip route add default via 10.0.1.1 dev eth1 2> /dev/null || true
    SHELL
  end

  # ----------------------------------------------------
  # 3. SERVER (The machine to be hardened)
  # ----------------------------------------------------
  config.vm.define "server" do |srv|
    srv.vm.box = "ubuntu/focal64"
    srv.vm.hostname = "hardened-server"
    
    # Connected to Server_Net. Firewall is .1, Server is assigned .10
    srv.vm.network "private_network", virtualbox__intnet: "Server_Net", ip: "10.0.2.10"
    
    config.vm.provider "virtualbox" do |vb|
      vb.name = "Lab_Server"
      vb.memory = "1024"
      vb.cpus = 1
    end

    # Set the firewall IP (10.0.2.1) as the default gateway for this subnet
    srv.vm.provision "shell", inline: <<-SHELL
      ip route del default via 10.0.2.2 2> /dev/null || true
      ip route add default via 10.0.2.1 dev eth1 2> /dev/null || true
    SHELL
  end

end