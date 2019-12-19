# -*- mode: ruby -*-
# vi: set ft=ruby :

image = "sbeliakou/centos"

Vagrant.configure("2") do |config|
    [
        { :hostname => 'master', :ip => '192.168.56.224', :ram => 2536 },
        { :hostname => 'worker', :ip => '192.168.56.225', :ram => 2536 }
    ].each do |node|
        config.vm.define node[:hostname] do |nodeconfig|
            nodeconfig.vm.box = image
            nodeconfig.vm.hostname = node[:hostname]
            nodeconfig.vm.network :private_network, ip: node[:ip]
            nodeconfig.ssh.insert_key = false

            nodeconfig.vm.provider :virtualbox do |vb|
                vb.name = node[:hostname]
                vb.memory = node[:ram] ? node[:ram] : "1536"
                vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
                vb.customize ["modifyvm", :id, "--cpuexecutioncap", "30"]
            end
            nodeconfig.vm.provision "file", source: "/home/student/.ssh/id_rsa.pub", destination: "/home/vagrant/.ssh/host.pub"
            nodeconfig.vm.provision 'shell' do |shell|
                shell.inline = 'cat /home/vagrant/.ssh/host.pub >> /home/vagrant/.ssh/authorized_keys'
            end
            if nodeconfig.vm.hostname == 'master'
                nodeconfig.vm.network "forwarded_port", guest: 6443, host: 6443
                nodeconfig.vm.network "forwarded_port", guest: 32500, host: 8080
            end
        end
    end
end