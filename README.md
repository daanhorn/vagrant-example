# vagrant-example
Vagrant can be used to automate the installation of a virtual machine. Multiple virtual machine providers are supported (e.g. VMWare, Hyper-V and Docker), but this guide will focus on VirtualBox.  

For more detailed information about Vagrant, please read the [docs](https://www.vagrantup.com/docs/)

# Running
This example needs Vagrant and VirtualBox installed.  

Go to https://www.vagrantup.com/downloads.html and download and use the installer to install Vagrant.  

Go to https://www.virtualbox.org/wiki/Downloads and download and use the installer to install VirtualBox.

To run the virtual machine, simply clone this repository and run `vagrant up` from the directory where the repository is cloned.

# Create virtual machine yourself
In Vagrant, virtual machines are based on `boxes`. A `box` is a base image, similar to a base image in Docker.  
  
Open Command Prompt and run:
```console
mkdir alliander_vm
cd alliander_vm
vagrant init box-cutter/ubuntu1604-desktop
```

A `Vagrantfile` will be created based on the box-cutter/ubuntu1604-desktop box from Hashicorp Atlas.  
More about the boxcutter ubuntu boxes and how to build the boxes using Packer can be found here: https://github.com/boxcutter/ubuntu
More boxes can be found there : https://atlas.hashicorp.com/boxes/search

To start the virtual machine, simply run:
```
vagrant up --provider virtualbox
```

This might take a while the first time, since the box has to be downloaded.  

# Synced folders
A synced folder is a folder which is accessible from the host and from the virtual machine, so that files can be shared between the both.  
By default, Vagrant creates a synced folder in the virtual machine in `/vagrant` which maps to the directory where the vagrant box is in (the alliander_vm folder in this case).

# Provisioning
A virtual machine can be provisioned with additional software by Vagrant. This can be done using a Shell script, but Pupper, Chef, Ansible, Salt and Docker are also supported.  
To provision Apache, create a shell script named `bootstrap.sh` like:
```
#!/usr/bin/env bash

apt-get update
apt-get install -y apache2
if ! [ -L /var/www ]; then
  rm -rf /var/www
  ln -fs /vagrant /var/www
fi
```

And add this to the `Vagrantfile` like:
```
Vagrant.configure("2") do |config|
  config.vm.box = "box-cutter/ubuntu1604-desktop"
  config.vm.provision :shell, path: "bootstrap.sh"
end
```

This will be executed when starting Vagrant using `vagrant up`. A running virtual machine can be provisioned using `vagrant reload --provision`.

# Useful links
- Vagrant docs: https://www.vagrantup.com/docs/
