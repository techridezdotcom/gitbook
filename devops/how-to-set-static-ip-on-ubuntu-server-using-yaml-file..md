# How to set static ip on ubuntu server using yaml file.

```
$ cd /etc/netplan/
```

You need check what file you have in this directory some have **01-netcfg.yaml**  or **50-cloud-init.yaml** that depends on you, for this tutorial i have **50-cloud-init.yaml**

so type **ls** to find whats inside ? &#x20;

{% code title="$ ls" %}
```bash
50-cloud-init.yaml 
```
{% endcode %}

so we have **50-cloud-init.yaml** in our directory, first i will backup this file, for that you need to type following commands, if i type **ls** again i can see backup file and current configuration file, before that i will login as root, for that i will use **sudo su**

```bash
$ sudo su
$ cp 50-cloud-init.yaml 50-cloud-init.yaml.backup
$ ls 
$ 50-cloud-init.yaml 50-cloud-init.yaml.backup
```

now i need to edit **50-cloud-init.yaml** , i will use nano to edit the file.&#x20;

```bash
$ nano 50-cloud-init.yaml
```

this will open **50-cloud-init.yaml** in nano. your ethernet adpter may vary, mine is **ens160**:&#x20;

```yaml
# This file is generated from information provided by
# the datasource.  Changes to it will not persist across an instance.
# To disable cloud-init's network configuration capabilities, write a file
# /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg with the following:
# network: {config: disabled}
#network:
#    ethernets:
#        ens160:
#            dhcp4: yes
#    version: 2
network:
  version: 2
  renderer: networkd
  ethernets:
    ens160:
     dhcp4: no
     addresses: [192.168.1.230/24]
     gateway4: 192.168.1.1
     nameservers:
       addresses: [8.8.8.8]

```

Now if you see above you can find i have commended default dhcp, and added my static ip, you can copy same settings and change to your settings if you want.&#x20;

&#x20;Now press ^x  (Ctrl + x ) to exit, and save it.

sudo netplan --debug apply

```bash
$ sudo netplan --debug apply
** (generate:1962): DEBUG: 12:48:51.528: Processing input file /etc/netplan/50-cloud-init.yaml..
** (generate:1962): DEBUG: 12:48:51.529: starting new processing pass
** (generate:1962): DEBUG: 12:48:51.530: ens160: setting default backend to 1
** (generate:1962): DEBUG: 12:48:51.530: Configuration is valid
** (generate:1962): DEBUG: 12:48:51.531: Generating output files..
** (generate:1962): DEBUG: 12:48:51.532: NetworkManager: definition ens160 is not for us (backend 1)
DEBUG:netplan generated networkd configuration changed, restarting networkd
DEBUG:no netplan generated NM configuration exists
DEBUG:ens160 not found in {}
DEBUG:Merged config:
network:
  bonds: {}
  bridges: {}
  ethernets:
    ens160:
      addresses:
      - 192.168.1.230/24
      dhcp4: false
      gateway4: 192.168.1.1
      nameservers:
        addresses:
        - 8.8.8.8
  vlans: {}
  wifis: {}

DEBUG:Skipping non-physical interface: lo
DEBUG:device ens160 operstate is up, not changing
DEBUG:{}
DEBUG:netplan triggering .link rules for lo
DEBUG:netplan triggering .link rules for ens160

```

