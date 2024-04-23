# Proxmox Command Guide

## Commands&#x20;

```
qm <command> <vmid> [OPTIONS]
qm [create|set] <vmid>
        --memory  <MBYTES>    memory in MB (64 - 8192)
        --smp  <N>            set number of CPUs to <N>
        --ostype NAME         specify OS type
        --onboot [yes|no]     start at boot
        --keyboard XX         set vnc keyboard layout
        --cpuunits <num>      CPU weight for a VM
        --name <text>         Add a name for the VM
        --boot [a|c|d|n]      specify boot order
        --bootdisk <disk>     enable booting from <disk>
        --acpi (yes|no)       enable/disable ACPI
        --localtime (yes|no)  set the RTC to local time

        --vlan[0-9u] MODEL=XX:XX:XX:XX:XX:XX[,MODEL=YY:YY:YY:YY:YY:YY]

        --ide<N>    [file=]file,][,media=d]
                    [,cyls=c,heads=h,secs=s[,trans=t]]
                    [,snapshot=on|off][,cache=on|off][,format=f]
        --ide<N> <GBYTES>     create new disk
        --ide<N> delete       delete disk
        --cdrom <file>        is an alias for --ide2 <file>,media=cdrom

        --scsi<N>   [file=]file,][,media=d]
                    [,cyls=c,heads=h,secs=s[,trans=t]]
                    [,snapshot=on|off][,cache=on|off][,format=f]
        --scsi<N> <GBYTES>    create new disk
        --scsi<N> delete      delete disk

        --virtio<N> [file=]file,][,media=d]
                    [,cyls=c,heads=h,secs=s[,trans=t]]
                    [,snapshot=on|off][,cache=on|off][,format=f]
        --virtio<N> <GBYTES>  create new disk
        --virtio<N> delete    delete disk

qm monitor <vmid>       connect to vm control monitor
qm start <vmid>         start vm
qm reboot <vmid>        reboot vm (shutdown, start)
qm shutdown <vmid>      gracefully stop vm (send poweroff)
qm stop <vmid>          kill vm (immediate stop)
qm reset <vmid>         reset vm (stop, start)
qm suspend <vmid>       suspend vm
qm resume <vmid>        resume vm
qm destroy <vmid>       destroy vm (delete all files)

qm cdrom <vmid> [<device>] <path>  set cdrom path. <device is ide2 by default>
qm cdrom <vmid> [<device>] eject   eject cdrom

qm unlink <vmid> <file>  delete unused disk images
qm vncproxy <vmid> <ticket>  open vnc proxy
qm list                 list all virtual machines
```

How to import qcow2 to proxmox ?

```bash
qm importdisk <vmid> yourimage.qcow2 namestoragepool
```
