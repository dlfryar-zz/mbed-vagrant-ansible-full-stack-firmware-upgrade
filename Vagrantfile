# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.synced_folder "~/", "/vagrant_home"

  # One vm for the development environment
  config.vm.define "devbox" do |devbox|
    devbox.vm.hostname = 'devbox'
    devbox.vm.box = "ubuntu/xenial64"    
    devbox.vm.network :private_network, ip: "192.168.10.5"
    
    devbox.ssh.insert_key = true
    devbox.ssh.forward_agent = true
    
    devbox.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 1
    end

    # Python is not installed on 16.04+ ubuntu/xenial64 by default and
    # Ansible won't run without it.
    devbox.vm.provision "shell",
      inline: "sudo apt-get install python -y",
      keep_color: true

    # Run the Ansible playbooks
    devbox.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning/ansible/playbooks/devbox.yml"
    end  
  end

  # One vm for the application server running the MEAN stack
  config.vm.define "server" do |server|
    server.vm.hostname = 'server'
    server.vm.box = "ubuntu/xenial64"
    
    server.vm.network :private_network, ip: "192.168.10.6"
    server.vm.network "forwarded_port", guest: 3000, host: 3000
    server.vm.network "forwarded_port", guest: 8080, host: 8080
    
    server.ssh.insert_key = true
    server.ssh.forward_agent = true
    
    server.vm.provider "virtualbox" do |vb|
      vb.memory = 2048
      vb.cpus = 1
    end

    # Python is not installed on 16.04+ ubuntu/xenial64 by default and
    # Ansible won't run without it.
    server.vm.provision "shell",
      inline: "sudo apt-get install python -y",
      keep_color: true

    # Run the Ansible playbooks
    server.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning/ansible/playbooks/server.yml"
    end  
  end
  
end
