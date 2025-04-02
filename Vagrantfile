# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV['VAGRANT_SERVER_URL'] = 'https://vagrant.elab.pro'
Vagrant.configure("2") do |config|

  config.vm.provision "file", source: "files/vagrant_test.pub", destination: "/home/vagrant/.ssh/"

  config.vm.define "controlnode" do |controlnode|
    controlnode.vm.box = "ubuntu/focal64"
    controlnode.vm.hostname = "controlnode"
    controlnode.vm.network "private_network", ip: "192.168.50.4"
    controlnode.vm.synced_folder "./ansible","/home/vagrant/ansible"
    controlnode.vm.synced_folder "./files","/home/vagrant/files/"
    controlnode.vm.provision "file", source: "files/vagrant_test", destination: "/home/vagrant/.ssh/"
    controlnode.vm.provision "shell", inline: <<-SHELL
      sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/#g' /etc/ssh/sshd_config
      service ssh restart
      sudo add-apt-repository ppa:ansible/ansible
      sudo apt update -y && sudo apt -y install sshpass ansible mc
      chmod 600 /home/vagrant/.ssh/vagrant_test
      chmod 644 /home/vagrant/.ssh/vagrant_test.pub
    SHELL
  end

  config.vm.define "semaphore_ubuntu" do |server_semaphore|
    server_semaphore.vm.box = "ubuntu/focal64"
    server_semaphore.vm.hostname = "semaphore"
    server_semaphore.vm.network "private_network", ip: "192.168.56.41"
    server_semaphore.vm.provision "shell", inline: <<-SHELL
      chmod 644 /home/vagrant/.ssh/vagrant_test.pub
      cat /home/vagrant/.ssh/vagrant_test.pub >> /home/vagrant/.ssh/authorized_keys
    SHELL
  end

end
