# -*- mode: ruby -*-
# vi: set ft=ruby :


Vagrant.configure("2") do |config|

  ## dns ##
  config.vm.define "dns" do |dns|
    dns.vm.box = "debian95_x64_ga"
    dns.vm.network "private_network", ip: "172.20.0.3", virtualbox__intnet: "intnet"
 
    dns.vm.provider "virtualbox" do |vb|
      vb.gui = false
      vb.memory = 768
      vb.cpus = 4
    end
        
    dns.vm.provision "shell", inline: <<-SHELL
      mkdir -p /home/vagrant/.ssh
      sudo chown vagrant:vagrant .ssh
      cat /vagrant/keys/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys
      sudo chown vagrant:vagrant /home/vagrant/.ssh/authorized_keys
    SHELL
  end


  ## host 1 ##
  config.vm.define "h1" do |h1|
    h1.vm.box = "debian95_x64_ga"
    h1.vm.network "private_network", ip: "172.20.0.100", virtualbox__intnet: "intnet"
 
    h1.vm.provider "virtualbox" do |vb|
      vb.gui = false
      vb.memory = 768
      vb.cpus = 4
    end
        
    h1.vm.provision "shell", inline: <<-SHELL
      mkdir -p /home/vagrant/.ssh
      sudo chown vagrant:vagrant .ssh
      cat /vagrant/keys/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys
      sudo chown vagrant:vagrant /home/vagrant/.ssh/authorized_keys
    SHELL
  end


  ## host 2 ##
  config.vm.define "h2" do |h2|
    h2.vm.box = "debian95_x64_ga"
    h2.vm.network "private_network", ip: "172.20.0.101", virtualbox__intnet: "intnet"
 
    h2.vm.provider "virtualbox" do |vb|
      vb.gui = false
      vb.memory = 768
      vb.cpus = 4
    end
    
    h2.vm.provision "shell", inline: <<-SHELL
      mkdir -p /home/vagrant/.ssh
      sudo chown vagrant:vagrant .ssh
      cat /vagrant/keys/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys
      sudo chown vagrant:vagrant /home/vagrant/.ssh/authorized_keys
    SHELL
  end


  ## man in the middle / sniffer ##
  config.vm.define "mitm" do |mitm|
    mitm.vm.box = "debian95_x64_ga"
    mitm.vm.network "private_network", ip: "172.20.0.4", virtualbox__intnet: "intnet"
    mitm.vm.hostname = "mitm"
 
    mitm.vm.provider "virtualbox" do |vb|
      vb.gui = false
      vb.memory = 1024
      vb.cpus = 4
      vb.customize ["modifyvm", :id, "--nicpromisc2", "allow-all"]
    end
        
    mitm.vm.provision "shell", inline: <<-SHELL
      mkdir -p /home/vagrant/.ssh
      sudo chown vagrant:vagrant .ssh
      cat /vagrant/keys/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys
      sudo chown vagrant:vagrant /home/vagrant/.ssh/authorized_keys
    SHELL
  end


  ## puppet master ##
  config.vm.define "pm" do |pm|
    pm.vm.box = "debian95_x64_ga"
    pm.vm.network "private_network", ip: "172.20.0.41", virtualbox__intnet: "intnet"
    pm.vm.synced_folder "./ansible", "/home/vagrant/pm"
 
    pm.vm.provider "virtualbox" do |vb|
      vb.gui = false
      vb.memory = 1024
      vb.cpus = 4
    end
    
    pm.vm.provision "shell", inline: <<-SHELL
      cp /vagrant/keys/id_rsa /home/vagrant/.ssh
      cp /home/vagrant/pm/dot_ssh/config /home/vagrant/.ssh
      sudo chown vagrant:vagrant /home/vagrant/.ssh/id_rsa
      sudo chown vagrant:vagrant /home/vagrant/.ssh/config
      sudo apt update
      sudo apt install -y ansible
      echo export ANSIBLE_HOSTS=/home/vagrant/pm/inventory.ini >> /home/vagrant/.bashrc
      sudo chown vagrant:vagrant /home/vagrant/.bashrc
    SHELL
  end

end
