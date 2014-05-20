# -*- mode: ruby -*-
# vi: set ft=ruby :

$provisioner = <<SCRIPT
  #!/bin/sh

  set -e -x

  sudo apt-get update -qq
  sudo apt-get install -qy python-software-properties
  sudo add-apt-repository -y ppa:ubuntu-toolchain-r/test
  sudo apt-get update -qq
  sudo apt-get install -qy build-essential curl git gcc-4.7 g++-4.7 libsnappy-dev libbz2-dev zlib1g-dev vim

  sudo rm /usr/bin/gcc && sudo ln -s /usr/bin/gcc-4.7 /usr/bin/gcc
  sudo rm /usr/bin/g++ && sudo ln -s /usr/bin/g++-4.7 /usr/bin/g++

  curl -s https://gflags.googlecode.com/files/gflags-2.0-no-svn-files.tar.gz | tar -xzv
  cd gflags-2.0/ && ./configure && make && sudo make install && cd .. && rm -rf gflags-2.0*

  # Go time.
  sudo curl -s https://storage.googleapis.com/golang/go1.2.2.linux-amd64.tar.gz | sudo tar -v -C /usr/local -xz
  echo "export PATH=/usr/local/go/bin:\\$PATH" >> .bashrc
  echo "export GOPATH=/vagrant_gopath" >> .bashrc
SCRIPT

Vagrant.require_version '>= 1.5.0'
Vagrant.configure("2") do |config|
  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder "../../../../", "/vagrant_gopath"

  config.vm.provision "shell", inline: $provisioner
end