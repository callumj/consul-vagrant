# -*- mode: ruby -*-
# vi: set ft=ruby :
$script = <<SCRIPT

sudo apt-get install -y unzip

echo Fetching Consul...
cd /tmp/
wget https://dl.bintray.com/mitchellh/consul/0.3.0_linux_amd64.zip -O consul.zip

echo Installing Consul...

unzip consul.zip
sudo mv consul /usr/bin/consul

SCRIPT

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/trusty64"

  config.vm.provision "shell", inline: $script

  config.vm.define "n1" do |n1|
    n1.vm.hostname = "n1"
    n1.vm.network "private_network", ip: "172.20.20.10"
    n1.vm.provision "shell", inline: "sudo chown vagrant /etc/init"
    n1.vm.provision "file", source: "shared/n1.upstart.conf", destination: "/etc/init/consul.conf"
    n1.vm.provision "shell", inline: "sudo service consul start"
  end

  config.vm.define "n2" do |n2|
    n2.vm.hostname = "n2"
    n2.vm.network "private_network", ip: "172.20.20.11"
    n2.vm.provision "shell", inline: "sudo chown vagrant /etc/init"
    n2.vm.provision "file", source: "shared/n2.upstart.conf", destination: "/etc/init/consul.conf"
    n2.vm.provision "shell", inline: "sudo service consul start"
    n2.vm.provision "shell", inline: "/usr/bin/consul join 172.20.20.10"
  end

  config.vm.define "n3" do |n3|
    n3.vm.hostname = "n3"
    n3.vm.network "private_network", ip: "172.20.20.12"
    n3.vm.provision "shell", inline: "sudo chown vagrant /etc/init"
    n3.vm.provision "file", source: "shared/n3.upstart.conf", destination: "/etc/init/consul.conf"
    n3.vm.provision "shell", inline: "sudo service consul start"
    n3.vm.provision "shell", inline: "/usr/bin/consul join 172.20.20.10"
  end
end
