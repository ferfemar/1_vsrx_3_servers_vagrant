# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.ssh.shell = '/bin/sh'
  config.ssh.insert_key = false
  
  config.vm.define "vsrx1" do |srx|
    srx.vm.box = "juniper/ffp-12.1X47-D15.4" 
    srx.vm.synced_folder ".", "/vagrant", disabled: true
    srx.vm.network "private_network", ip: "10.10.10.1", nic_type: 'virtio', virtualbox__intnet: "srv1net"
    srx.vm.network "private_network", ip: "10.20.20.1", nic_type: 'virtio', virtualbox__intnet: "srv2net" 
    srx.vm.network "private_network", ip: "10.30.30.1", nic_type: 'virtio', virtualbox__intnet: "srv3net" 
    srx.vm.provider "virtualbox" do |v|
     v.check_guest_additions = false
    end
    srx.vm.provider "virtualbox" do |v|
     v.customize ["modifyvm", :id, "--memory", "512"]
    end
    srx.vm.provision "file", source: "configs/initial.cfg", destination: "/cf/root/initial.cfg"
    # srx.vm.provision "file", source: "configs/configure.sh", destination: "/cf/root/configure.sh"
    # srx.vm.provision "shell", inline: "/bin/sh /cf/root/configure.sh", privileged: false
    srx.vm.provision :host_shell do |host_shell|
     host_shell.inline = 'vagrant ssh vsrx1 -c "/usr/sbin/cli -f /cf/root/initial.cfg"'
    end
  end

  config.vm.define "srv1" do |srv|
    srv.vm.box = "olbat/tiny-core-micro"
    srv.vm.synced_folder ".", "/vagrant", disabled: true
    srv.vm.network "private_network", ip: "10.10.10.2",
      netmask: "255.255.255.0", virtualbox__intnet: "srv1net"
    srv.vm.provider "virtualbox" do |v|
     v.customize ["modifyvm", :id, "--memory", "64"]
    end 
    srv.vm.provider "virtualbox" do |v|
     v.check_guest_additions = false
    end
    srv.vm.provision "shell", inline: "route add -net 10.0.0.0 netmask 255.0.0.0 gw 10.10.10.1", run: "always"
    srv.vm.provision "shell", inline: "route add -net 172.16.0.0 netmask 255.240.0.0 gw 10.10.10.1", run: "always"
    srv.vm.provision "shell", inline: "route add -net 192.168.0.0 netmask 255.255.0.0 gw 10.10.10.1", run: "always"
  end
  
  config.vm.define "srv2" do |srv|
    srv.vm.box = "olbat/tiny-core-micro"
    srv.vm.synced_folder ".", "/vagrant", disabled: true
    srv.vm.network "private_network", ip: "10.20.20.2",
      netmask: "255.255.255.0", virtualbox__intnet: "srv2net"
    srv.vm.provider "virtualbox" do |v|
     v.customize ["modifyvm", :id, "--memory", "64"]
    end
    srv.vm.provision "shell", inline: "route add -net 10.0.0.0 netmask 255.0.0.0 gw 10.20.20.1", run: "always"
    srv.vm.provision "shell", inline: "route add -net 172.16.0.0 netmask 255.240.0.0 gw 10.20.20.1", run: "always"
    srv.vm.provision "shell", inline: "route add -net 192.168.0.0 netmask 255.255.0.0 gw 10.20.20.1", run: "always"
  end
  
  config.vm.define "srv3" do |srv|
    srv.vm.box = "olbat/tiny-core-micro"
    srv.vm.synced_folder ".", "/vagrant", disabled: true
    srv.vm.network "private_network", ip: "10.30.30.2",
      netmask: "255.255.255.0", virtualbox__intnet: "srv3net"
    srv.vm.provider "virtualbox" do |v|
     v.customize ["modifyvm", :id, "--memory", "64"]
    end
    srv.vm.provider "virtualbox" do |v|
     v.check_guest_additions = false
    end
    srv.vm.provision "shell", inline: "route add -net 10.0.0.0 netmask 255.0.0.0 gw 10.30.30.1", run: "always"
    srv.vm.provision "shell", inline: "route add -net 172.16.0.0 netmask 255.240.0.0 gw 10.30.30.1", run: "always"
    srv.vm.provision "shell", inline: "route add -net 192.168.0.0 netmask 255.255.0.0 gw 10.30.30.1", run: "always"
  end

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
end
