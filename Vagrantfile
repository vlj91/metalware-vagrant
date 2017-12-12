# -*- mode: ruby -*-
# vi: set ft=ruby :

$script = <<SCRIPT
yum install -y vim

mkdir -p /root/.ssh
cat /home/vagrant/.ssh/authorized_keys > /root/.ssh/authorized_keys

curl -sL http://git.io/metalware-installer | /bin/bash

echo "pathmunge /opt/metalware/opt/ruby/bin" > /etc/profile.d/metalware-ruby.sh
. /etc/profile

cd /tmp/metalware
bundle install

SCRIPT

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "hfm4/centos7"
  config.vm.box_check_update = false
  config.vm.network "private_network", ip: "192.168.50.101"

  config.ssh.forward_agent = true
  config.ssh.username = "vagrant"
  config.ssh.private_key_path = ["~/.ssh/id_rsa", "~/.vagrant.d/insecure_private_key"]
  config.ssh.insert_key = false
  config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/authorized_keys"

  config.vm.provision "shell",
    inline: $script,
    env: {
      alces_SOURCE_BRANCH: "develop",
      alces_OS: "el7"
    }

  config.vm.synced_folder "../metalware", "/tmp/metalware"
  config.vm.synced_folder "~/Workspace/commander/", "/tmp/commander"
  config.vm.synced_folder "~/Workspace/ruby-pcap", "/tmp/ruby-pcap"
  config.vm.synced_folder "~/Workspace/metalware-default", "/var/lib/metalware/repo"
end
