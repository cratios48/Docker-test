# -*- mode: ruby -*-
# vi: set ft=ruby :

BOX_IMAGE = 'ubuntu/xenial64'
NODE_COUNT = 1

Vagrant.configure("2") do |config|
  (1..NODE_COUNT).each do |i|
    config.vm.define "node-#{i}" do |subconfig|
      subconfig.vm.box = BOX_IMAGE
      subconfig.vm.hostname = "docker-#{i}"
      subconfig.vm.network "private_network", ip: "10.10.10.#{ i * 10 }"
      subconfig.vm.provider "virtualbox" do |v|
        v.cpus = 2
        v.memory = 2048
      end
    end
  end

  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get -y install vim
    sudo apt-get -y remove docker docker-engine docker.io
    sudo apt-get update
    sudo apt-get -y install apt-transport-https ca-certificates curl software-properties-common
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    sudo apt update
    sudo apt-get install -y docker-ce
    sudo usermod -aG docker vagrant
  SHELL
end