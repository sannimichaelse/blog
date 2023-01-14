# How to Install and Configure a CentOS Virtual Machine on macOS using Vagrant

# Introduction 

Vagrant is a command-line tool for building and managing virtualized development environments. By default, Vagrant can provision machines on top of VirtualBox, Hyper-V, and Docker. Support for other providers such as Libvirt (KVM), VMware and AWS can be enabled via the Vagrant plugin system.

It is often used in software development to ensure all team members are building for the same configuration. Not only does it share environments, but it also shares code as well. This allows the code from one developer to work on the system of another, making collaborative and cooperative development possible.

In this tutorial, you will learn how to install and Configure Vagrant on MacOS. You will also learn how to set up a Virtual Machine using CentOS 8 as well as show you how to create a development environment.
Prerequisites
Make sure you have Virtual Box Installed. We will be using Oracle Virtual Box For this tutorial

# Step 1 - Getting Started with Vagrant
## Installation

- To find the latest version of Vagrant, use a web browser to navigate to its official webpage:
https://www.vagrantup.com/downloads.html

- You will see a list of all the different supported operating systems, with a 32-bit and 64-bit package for each. Download for MacOS operating system, then run the installer.

**NB**: The vagrant installer will automatically add vagrant to your system path so that it is available in terminals. If it is not found, please try logging out and logging back in to your system (this is particularly necessary sometimes for Windows).

- There are two ways to check if the installation was successful: You can either use:

```
vagrant -v
```

which should show the version number running on your computer. The latest version to date is vagrant 2.2.7

Or you can type in the following command in the terminal:

```
vagrant
```

This output would show you a list of frequently used commands if the tool were installed correctly.

# Step 2 - Vagrant Project Setup

Now that you have Vagrant installed on your system, let’s create a development environment using the VirtualBox provider, which is the default provider for Vagrant. 

- The first step is to create a directory that will be the project root directory. Create the project directory and switch to it: 

```
mkdir -p ~/devops/vagrant 
cd ~/devops/vagrant 
```

- Download the  [CentOS 8 Box ](https://app.vagrantup.com/centos/boxes/8) from  [Vagrant box catalog](https://app.vagrantup.com/boxes/search)  and create a basic Vagrantfile with:

```
vagrant init centos/8
```

You can also specify the version of box to install

```
vagrant init centos/8 \--box-version 1905.1
```

When you run the init command, Vagrant installs the box to the current directory. The Vagrantfile is placed in the same directory and can be edited or copied.

## Vagrant Boxes

The basic unit in a Vagrant setup is called a “box” or a “Vagrant Box.” This is a complete, self-contained image of an operating system environment.

A Vagrant box is a clone of a base operating system image. Using a clone speeds up the launching and provisioning process.

- Instead of using the init command above, you can simply download and add a box with the command:

```
vagrant box add centos/8
```

This downloads the box and stores it locally.

- Next, you need to configure the Vagrantfile for the virtual box it will serve. Open the Vagrantfile with the command:

```
sudo vi Vagrantfile
```

- Once the Vagrantfile is open, change the config.vm.box string from “base” to “centos/8”.

```
config.vm.box = “centos/8”
```

You can add another line above the end command to specify a box version:

```
config.vm.box_version = “1905.1”
```

Or you can specify a URL to link directly to the box:

```
config.vm.box_url = “https://app.vagrantup.com/centos/boxes/8”
```

If you’d like to remove a box, use the following:

```
vagrant box remove centos/8 
```

## Provisioning

Provisioners in Vagrant allow you to automatically install software, alter configurations, and more on the machine as part of the vagrant up process.

Vagrant supports automatic provisioning through a bootstrap.sh file saved in the same directory as the Vagrantfile or using an inline shell.

```
Vagrant.configure("2") do |config|
  # ... other configuration

  config.vm.provision "shell", inline: "echo hello"
end
```

When you run the vagrant up command, it will echo hello to the terminal

## Providers

This tutorial shows you how to use Vagrant with VirtualBox. However, Vagrant can also work with many other backend providers. To launches Vagrant using VMware run the command:

```
vagrant up -provider=vmware_fusion 
```

Or you can launch Vagrant using Amazon Web Services with:

```
vagrant up -provider=aws 
```

Once the initial command is run, subsequent commands will apply to the same provider.

# Step 3 - Launching and Connecting to VM using vagrant SSH

## Vagrant Up
The main command to launch your new virtual environment is:

```
vagrant up
```

This will run the software and start a virtual Ubuntu environment quickly. However, even though the virtual machine is running, you will not see any output. Vagrant does not give any kind of user interface.

## Vagrant SSH
You can connect to your virtual machine (and verify that it is running) by using an SSH connection:

```
vagrant ssh
```

This opens a secure shell connection to the new virtual machine. Your command prompt will change to vagrant@host to indicate that you are logged into the virtual machine.

Once you are done exploring the virtual machine, you can exit the session with CTRL-D. The virtual machine will still be running in the background, but the SSH connection will be closed.

To stop the virtual machine from running, enter:

```
vagrant destroy
```

The file you downloaded will remain, but anything that was running inside the virtual machine will be gone.

## Networking
Vagrant includes options to place your virtual machine on a network. At the end of your Vagrantfile, just before the end command, use the config.vm.network command to specify network settings.

For example:

```
config.vm.network “forwarded_port”, guest: 80, host: 8080
```

For the changes to take place, save and reload Vagrant with the command:

```
vagrant reload
```

This creates a forwarded port for the guest system. You may also define private networks, public networks, and other more advanced options.

## Vagrant Share
Vagrant has a handy feature that you can use to share your Vagrant environment using a custom URL.

With your Vagrant environment running, use the following command:

```
vagrant share
```

The system will create a Vagrant Share session, then generate a URL. This URL can be copied and sent to another person. If you have Apache configured in your Vagrant session, anyone who uses this URL can see your Apache configuration page. This URL changes as you modify the contents of your shared folder.

You can close the sharing session with CTRL-C.

For more information, see the Vagrant Sharing documentation.

## Clean Up Vagrant
Once you are done working on your guest system, you have a few options on how to end the session.

- To stop the machine and save its current state-run:

```
vagrant suspend
```

You can resume by running vagrant up again. This is much like putting the machine in sleep mode.

- To shut down the virtual machine use the command:

```
vagrant halt
```

Again, vagrant up will reboot the same virtual machine, and you can resume where you left off. This is much like shutting down a regular machine.

- To remove all traces of the virtual machine from your system type in the following:

```
vagrant destroy
```
Anything you have saved in the virtual machine will be removed. This frees up the system resources used by Vagrant.

The next time you vagrant up, the machine will have to be re-imported and re-provisioned. This is much like formatting a hard drive on a system, then reloading a fresh image.

## Conclusion
By now, you should be familiar with the essential Vagrant operations. If you followed along with this tutorial for Vagrant, you might even have a virtual OS running right now!


