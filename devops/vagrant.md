# Vagrant

## How to install Vagrant on Manjaro

```
$ sudo pacman -S vagrant
```

{% hint style="info" %}
&#x20;After that install virtual-box
{% endhint %}

Probably you will get an error to run vagrant up, The vboxdrv kernel module is not loaded.

#### Symptoms:

{% code title="" %}
```bash
$ VBoxManage --version
WARNING: The vboxdrv kernel module is not loaded. Either there is no module
         available for the current kernel (5.4.28-1-MANJARO) or it failed to
         load. Please recompile the kernel module and install it by
 
           sudo /sbin/vboxconfig
 
         You will not be able to start VMs until this problem is fixed.
```
{% endcode %}

#### Solution:

Arch-Linux, Manjaro

```bash
# look for actual 
sudo pacman -Ss virtualbox-host-modules
# install 
sudo pacman -S linux54-virtualbox-host-modules
# add module
sudo modprobe vboxdrv
```

### Virtualbox have no access to USB devices

###

#### Reason

```bash
# ls -ls /dev/  | grep vboxusb
0 drwxr-x---  3 root vboxusers       60 Apr  6 09:14 vboxusb
```

#### Solution

```bash
usermod -a -G vboxusers {YOUR_ACCOUNT}
```

## How to add user name and password to vagrant file

Vagrant has a few options (see full doc [https://docs.vagrantup.com/v2/vagrantfile/ssh\_settings.html](https://docs.vagrantup.com/v2/vagrantfile/ssh\_settings.html)) :

```bash
Vagrant.configure("2") do |config|
  config.ssh.username = "user"
  config.ssh.password = "password"
end
```

**note** indeed, you need to make sure those users exist on the guest os (generally most vagrant box are created with vagrant user)

To have the connection between your different VMs, you can easily do that if you assign fix IP to the VM.

```
Vagrant.configure("2") do |config|
  config.vm.network :private_network, ip: "192.168.45.15"
end
```

when you connect to your second VM, you can run `ssh vagrant@192.168.45.15` and it will ssh to the first VM

#### How to add public IP dhcp to vagrantfile

```
config.vm.network "public_network",
use_dhcp_assigned_default_route: true
```

## Fix vagrant-hostmanager 127.0.0.1 with docker

{% embed url="https://ilhicas.com/2018/04/04/Vagrant-hostmanager-defaults-127.0.0.1.html" %}



### How to Configure vagrant box names

* Virtualbox GUI Name
* vm hostname
* vagrant status name

```
config.vm.box = "bento/ubuntu-18.04"
  config.vm.hostname = "vmhostname"
        config.vm.define "showsvagrantupboxname"
        config.vm.provider :virtualbox do |vb|
            vb.name = "ShowsVirtualboxGUIname"
            end
```

Change "vmhostname" to your desired _`hostname`_

Change "showvagrantupboxname" to your desired box name, this will show in the vagrant status&#x20;

change "ShowVirtualboxGUIname" this will show in the Virtual box GUI

