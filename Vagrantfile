# -*- mode: ruby -*-
# vi: set ft=ruby :

# require a Vagrant recent version
Vagrant.require_version ">= 2.2.0"

Vagrant.configure("2") do |config|

  # Alpine Linux box
  config.vm.box = "boxomatic/alpine-3.16"
  config.vm.box_check_update = false

  # VBoxGuestAdditions auto update
  config.vbguest.auto_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Share additional folders to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder.
  config.vm.synced_folder ".", "/vagrant"
  config.vm.synced_folder "html", "/var/www/html"

  config.vm.provider "virtualbox" do |vb|
     # Customize the amount of memory on the VM:
     vb.memory = "512"
  end

  # Install Apache with a shell script
  config.vm.provision "shell", inline: <<-SHELL
    apk update
    apk add apache2
    sed -i 's|localhost/htdocs|html|' /etc/apache2/httpd.conf
    rc-service apache2 start
    rc-update add apache2
  SHELL
end
