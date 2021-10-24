# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
    config.vm.define "centos" do |centos|
        centos.vm.hostname = "centos.example.com"
        centos.vm.box = "generic/centos7"
        centos.vm.network "private_network", ip: "10.10.10.19"
        centos.vm.synced_folder ".", "/vagrant", type: "rsync"
        centos.vm.provision "shell", inline: <<-SHELL
            # Setup DNS client with vagranting-dns
            ETH0=$(sudo nmcli connection show | grep eth0 | cut -d ' ' -f 4)
            ETH1=$(sudo nmcli connection show | grep eth1 | cut -d ' ' -f 4)
            sudo nmcli connection modify $ETH1 ipv4.dns 10.10.10.2
            sudo nmcli connection modify $ETH1 ipv4.dns-search example.com
            sudo nmcli connection modify $ETH1 ipv4.dns-priority 1
            sudo nmcli connection modify $ETH0 ipv4.dns-priority 10
            sudo nmcli connection up $ETH1
            sudo nmcli connection up $ETH0
        SHELL
        centos.vm.provider "virtualbox" do |vb|
            vb.cpus = "1"
            vb.memory = "1024"
            vb.customize ["modifyvm", :id, "--groups", "/vagranting"] #If you have problems with folder exist error -> Comment out this line
            vb.name = "centos"
        end
    end
end

