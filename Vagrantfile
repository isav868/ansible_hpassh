Vagrant.configure("2") do |config|

  config.ssh.insert_key = false
  config.vm.boot_timeout = 800
  config.vm.provision "file", source: "ssh/onix_ansible_ed25519.pub", destination: "~/.ssh/authorized_keys"

  #config.vm.define "debian9" do |debian9|
  #  debian9.vm.box = "generic/debian9"
  #  debian9.vm.network "private_network", ip: "172.28.128.9"
  #end

  #config.vm.define "debian10" do |debian10|
  #  debian10.vm.box = "generic/debian10"
  #  debian10.vm.network "private_network", ip: "172.28.128.10"
  #end

  #config.vm.define "ubuntu14" do |ubuntu14|
  #  ubuntu14.vm.box = "ubuntu/trusty64"
  #  ubuntu14.vm.network "private_network", ip: "172.28.128.14"
  #end

  #config.vm.define "ubuntu16" do |ubuntu16|
  #  ubuntu16.vm.box = "ubuntu/xenial64"
  #  ubuntu16.vm.network "private_network", ip: "172.28.128.16"
  #end

  #config.vm.define "ubuntu18" do |ubuntu18|
  #  ubuntu18.vm.box = "ubuntu/bionic64"
  #  ubuntu18.vm.box = "hashicorp/bionic64"
  #  ubuntu18.vm.network "private_network", ip: "172.28.128.18"
  #end

  config.vm.define "ubuntu20" do |ubuntu20|
    ubuntu20.vm.box = "ubuntu/focal64"
    ubuntu20.vm.network "private_network", ip: "172.28.128.20"
  end

end

