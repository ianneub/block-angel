# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "opscode_ubuntu-14.04"
  config.vm.box_url = "http://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_ubuntu-14.04_chef-provisionerless.box"

  config.vm.network "forwarded_port", guest: 80, host: 8000
  config.vm.network "forwarded_port", guest: 25565, host: 25565

  config.vm.provider "virtualbox" do |v|
    v.memory = 1024
    v.cpus = 2
  end

  config.vm.provision "shell", inline: <<-END
    # ansible
    apt-get update \
      && apt-get install -y software-properties-common \
      && apt-add-repository -y ppa:ansible/ansible \
      && apt-get update \
      && apt-get install -y ansible
  END

  config.vm.provision "shell", run: "always", inline: <<-END
    # ansible-playbook
    cd /vagrant \
      && ansible-playbook -i localhost.yml playbook.yml
  END

  config.vm.synced_folder ".", "/vagrant", mount_options: ["dmode=777,fmode=666"]
  # config.vm.synced_folder "/some/folder", "/vagrant/minecraft", mount_options: ["dmode=777,fmode=666"]
  # config.vm.synced_folder "/some/other/folder", "/vagrant/minecraft-www", mount_options: ["dmode=777,fmode=666"]
end
