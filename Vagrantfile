# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "bento/centos-7.2"

  config.vm.provision "shell", privileged: true, inline: <<-SHELL
    yum install -y epel-release
    yum update -y
    yum install -y \
             bzip2\
             kernel kernel-tools\
             python-devel\
             python-virtualenv\
             python2-pip\
             make\
             npm\
             java-1.8.0-openjdk\
             maven\
             unzip\
             git
    pip install pip --upgrade
    pip install awscli --user
  SHELL
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    if [ ! -d localstack ]; then
      git clone https://github.com/atlassian/localstack.git
    fi
    cd localstack
    make install
    make test
    make infra
  SHELL

  for port in 4567..4582 do
    config.vm.network "forwarded_port", guest: port, host: port
  end
end
