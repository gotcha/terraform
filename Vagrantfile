# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

$script = <<SCRIPT
SRCROOT="/opt/go"

# Install Go
sudo apt-get update
sudo apt-get install -y build-essential mercurial
sudo hg clone -u release https://code.google.com/p/go ${SRCROOT}
cd ${SRCROOT}/src
sudo ./all.bash

# Setup the GOPATH
sudo mkdir -p /opt/gopath
cat <<EOF >/tmp/gopath.sh
export GOPATH="/opt/gopath"
export PATH="/opt/go/bin:\$GOPATH/bin:\$PATH"
EOF
sudo mv /tmp/gopath.sh /etc/profile.d/gopath.sh
sudo chmod 0755 /etc/profile.d/gopath.sh

# Make sure the gopath is usable by vagrant
sudo chown -R vagrant:vagrant $SRCROOT
sudo chown -R vagrant:vagrant /opt/gopath

# Install some other stuff we need
sudo apt-get install -y curl git-core zip
SCRIPT

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "chef/ubuntu-10.04"

  config.vm.provision "shell", inline: $script

  ["vmware_fusion", "vmware_workstation"].each do |p|
    config.vm.provider "p" do |v|
      v.vmx["memsize"] = "2048"
      v.vmx["numvcpus"] = "2"
      v.vmx["cpuid.coresPerSocket"] = "1"
    end
  end
end
