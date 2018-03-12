# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # Specify shell path on the Tiny Core OS 
  config.ssh.shell = '/bin/sh'
  # Use insecure SSH key
  config.ssh.insert_key = false
  
  config.vm.define "vsrx1" do |srx|
    srx.vm.box = "juniper/ffp-12.1X47-D15.4" 
    srx.vm.synced_folder ".", "/vagrant", disabled: true
    # basic network config is provisioned using vagrant-junos plugin 
    srx.vm.network "private_network", ip: "10.10.10.1", nic_type: 'virtio', virtualbox__intnet: "srv1net"
    srx.vm.network "private_network", ip: "10.20.20.1", nic_type: 'virtio', virtualbox__intnet: "srv2net" 
    srx.vm.network "private_network", ip: "10.30.30.1", nic_type: 'virtio', virtualbox__intnet: "srv3net" 
    srx.vm.provider "virtualbox" do |v|
     v.check_guest_additions = false
    end
    # 512 MB RAM minimum, raise if you are experiencing issues
    srx.vm.provider "virtualbox" do |v|
     v.customize ["modifyvm", :id, "--memory", "512"]
    end
    # provisioning
    srx.vm.provision "file", source: "configs/initial.cfg", destination: "/cf/root/initial.cfg"
    srx.vm.provision :host_shell do |host_shell|
     host_shell.inline = "vagrant ssh vsrx1 -c 'cli -f /cf/root/initial.cfg'"
    end
  end

  #servers
  config.vm.define "srv1" do |srv|
    srv.vm.box = "olbat/tiny-core-micro"
    srv.vm.synced_folder ".", "/vagrant", disabled: true
    srv.vm.network "private_network", ip: "10.10.10.2",
      netmask: "255.255.255.0", virtualbox__intnet: "srv1net"
    # 64MB RAM seems to ideal
    srv.vm.provider "virtualbox" do |v|
     v.customize ["modifyvm", :id, "--memory", "64"]
    end 
    srv.vm.provider "virtualbox" do |v|
     v.check_guest_additions = false
    end
    # provisioning is set to always happen - Tiny Core changes do not persist
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

end
