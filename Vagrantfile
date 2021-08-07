# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV['VAGRANT_NO_PARALLEL'] = 'yes'

Vagrant.configure(2) do |config|

  config.vm.box = "generic/ubuntu2004"
  config.vm.provision "file", source: "./ansible_ssh_key.pub", destination: "/home/vagrant/.ssh/ansible_ssh_key.pub"
  config.vm.provision "file", source: "./ansible_ssh_key", destination: "/home/vagrant/.ssh/ansible_ssh_key"

  # Kubernetes Master Server
  config.vm.define "kmaster" do |node|
  
    node.vm.box_check_update  = false
    node.vm.box_version       = "3.2.18"
    node.vm.hostname          = "kmaster.example.com"

    node.vm.network "private_network", ip: "172.16.28.100"

  end


  # Kubernetes Worker Nodes
  NodeCount = 2

  (1..NodeCount).each do |i|

    config.vm.define "kworker#{i}" do |node|

      node.vm.box_check_update  = false
      node.vm.box_version       = "3.2.18"
      node.vm.hostname          = "kworker#{i}.example.com"

      node.vm.network "private_network", ip: "172.16.28.11#{i}"
      
    end

  end

  config.vm.provision "shell", inline: <<-'SCRIPT'
    cat /home/vagrant/.ssh/ansible_ssh_key.pub >> /home/vagrant/.ssh/authorized_keys
    sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config
  SCRIPT

end
