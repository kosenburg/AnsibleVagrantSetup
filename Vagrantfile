# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.provision "shell", inline: "yum -y update"
  config.vm.provision "shell", inline: "yum install -y epel-release"
  
  config.vm.define "worker1" do |worker1|
    worker1.vm.box = "centos/7"
    worker1.vm.network :private_network, ip: "192.168.50.6"
    worker1.vm.hostname = "worker1"
  end

  config.vm.define "worker2" do |worker2|
    worker2.vm.box = "centos/7"
    worker2.vm.network :private_network, ip: "192.168.50.5"
    worker2.vm.hostname = "worker2"
  end

  config.vm.define "master", primary: true do |master|
    master.vm.box = "centos/7"
    master.vm.network :private_network, ip: "192.168.50.4"
    master.vm.hostname = "master"
    master.vm.provision "shell", inline: "yum install -y ansible git sshpass"
  end

  config.vm.provision "file", source: "id_rsa", destination: "/home/vagrant/.ssh/id_rsa"
  public_key = File.read("id_rsa.pub")
  config.vm.provision "shell", inline: <<-SCRIPT
      chmod 600 /home/vagrant/.ssh/id_rsa
      echo 'Copying ansible-vm public SSH Keys to the VM'
      #mkdir -p /home/vagrant/.ssh
      chmod 700 /home/vagrant/.ssh
      echo '#{public_key}' >> /home/vagrant/.ssh/authorized_keys
      chmod -R 600 /home/vagrant/.ssh/authorized_keys
      echo 'Host 192.168.*.*' >> /home/vagrant/.ssh/config
      echo 'StrictHostKeyChecking no' >> /home/vagrant/.ssh/config
      echo 'UserKnownHostsFile /dev/null' >> /home/vagrant/.ssh/config
      chmod -R 600 /home/vagrant/.ssh/config
      SCRIPT
end
