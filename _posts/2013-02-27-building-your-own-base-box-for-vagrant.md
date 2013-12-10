---
layout: post
title: Building your own Base Box for Vagrant
date: '2013-02-27T00:30:00+08:00'
tags: [vagrant, veewee, basebox]
---

Here are steps on creating your own base box for Vagrant. I'll be using a Debian linux distro.




**Note**: Try [VeeWee](https://github.com/jedi4ever/veewee) If you haven't. It eases the building of Vagrant boxes.

## Requirements

- [Get a copy](http://www.debian.org/CD/) of a debian release. I chose `debian-6.0.6-i386-CD-1.iso`. After downloading, don't extract it.
- Vagrant depends on Oracle’s VirtualBox to create all of its virtual environments, so [download](https://www.virtualbox.org/wiki/Downloads) it as well and install.

## Getting Started

1. Launch Oracle VM VirtualBox Manager and create a new machine. *I named it "Debby"*. Make sure you **allocate enough hard disk space** because you won't be able to modify that later. _I set mine to 8GB._
2. Open the machine settings and click on `Storage` on the left. Select the empty under `Controller: IDE`. Beside the dropdown, click the icon to select your ISO image you just downloaded.
3. Start the VM and Install

## Convention over Configuration

As vagrant recommends, follow these if possible:

    Hostname: vagrant-debian
    Domain: vagrantup.com
    Root Password: vagrant
    Main account login: vagrant
    Main account password: vagrant

## Permissions &amp; Guest Additions

After installing:

    $ su
    Password:
    $ apt-get install sudo
    $ visudo

&gt; You may need to change your apt list


Append these 3 lines:

    Defaults    env_keep="SSH_AUTH_SOCK"
    %admin ALL=(ALL) ALL
    %admin ALL=NOPASSWD: ALL

    $ groupadd admin
    $ usermod -a -G admin vagrant

Reboot the VM

    $ sudo -i
    $ apt-get autoremove virtualbox-ose-guest-dkms virtualbox-ose-guest-utils virtualbox-ose-guest-x11
    $ apt-get install linux-headers-$(uname -r) build-essential

On the menu list, select **Devices** &gt; **Install Guest Additions**.

    $ mount /dev/cdrom /media/cdrom
    $ sh /media/cdrom/VBoxLinuxAdditions.run

Reboot the VM

    $ su -
    $ apt-get install ruby rubygems puppet openssh-server openssh-client
    $ echo 'UseDNS no' &gt;&gt; /etc/ssh/sshd_config
    $ gem install --no-rdoc --no-ri chef
    $ echo "deb http://apt.opscode.com/ squeeze-0.10 main" &gt; /etc/apt/sources.list.d/opscode.list
    $ apt-get update
    $ apt-get install opscode-keyring
    $ apt-get install chef
    $ exit
    # you're now user: Vagrant
    $ mkdir .ssh
    $ wget --no-check-certificate -O .ssh/authorized_keys https://raw.github.com/mitchellh/vagrant/master/keys/vagrant.pub
    $ chmod 700 .ssh
    $ chmod 600 .ssh/authorized_keys
    $ sudo apt-get clean

Shutdown

## Build Vagrant .box File

    $ vagrant package --base Debby
    [Debby] Clearing any previously set forwarded ports...
    [Debby] Creating temporary directory for export...
    [Debby] Exporting VM...
    [Debian] Compressing package to: package.box

    $ mv package.box vagrant-debian-squeeze32.box

----

## Use your Vagrant base box

    $ vagrant box add my_box vagrant-debian-squeeze32.box
    [vagrant] Downloading with Vagrant::Downloaders::File...
    [vagrant] Copying box to temporary location...
    [vagrant] Extracting box...
    [vagrant] Verifying box...
    [vagrant] Cleaning up downloaded box...
    $ mkdir testEnvironment
    $ cd testEnvironment
    $ vagrant init my_box
    $ vagrant up


Resources:

- [Create a Vanilla Debian Squeeze Vagrant Base Box | Brian Fisher](http://brianfisher.name/content/create-vanilla-debian-squeeze-vagrant-base-box)
- [Debian Sources List Generator](http://debgen.simplylinux.ch/)
- [Vagrant Documentation - Documentation - Base Boxes](www.vagrantup.com/v1/docs/base_boxes.html)
