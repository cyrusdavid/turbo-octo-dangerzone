---
title: "How to Disable VT-X On Virtualbox/Vagrant"
layout: post
tags: [vagrant,virtualbox,virtualization,intel,old hardware]
image:
  feature: vtx.png
---

When your hardware is old and does not support virtualization you might have come across this error:

> VT-x is not available (VERR_VMX_NO_VMX)

Here's how to fix it!


[![fix: VT-x is not available (VERR_VMX_NO_VMX)](http://i.imgur.com/DCFY9yn.png)](http://i.imgur.com/DCFY9yn.png){:rel='nofollow'}

## Vagrant

On vagrant, open up your `Vagrantfile` and add this lines inside the `Vagrant.configure` block:

{%highlight ruby%}
config.vm.provider :virtualbox do |vb|
  vb.customize ["modifyvm", :id, "--hwvirtex", "off"]
end
{%endhighlight%}

## Virtualbox

If you have virtualbox alone, open up your command line (<kbd>win</kbd>+<kbd>r</kbd> and type `cmd`) and enter the following commands:

{%highlight text%}
vboxmanage modifyvm "VIRTUALBOX_VM_NAME" --hwvirtex off
{%endhighlight%}

Modify `VIRTUALBOX_VM_NAME` with your virtual machine's name.

[![Virtualbox how to fix VT-x](http://i.imgur.com/FroyEt5.png)](http://i.imgur.com/FroyEt5.png){:rel='nofollow'}

After doing so you will successfully boot your VM with VT-x now disabled.
