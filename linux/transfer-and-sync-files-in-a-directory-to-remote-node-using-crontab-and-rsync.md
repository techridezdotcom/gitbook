# 🗃️ Transfer & sync files in a directory to remote node using crontab & rsync

1. openssh server
2. rsync
3. crontab

for testing type&#x20;

```sh
rsync -av -e ssh /backupsource/ root@destinationip:/backupdestination/
```

you need to provide remote server credentials, if this is working you can automate this using a bash file and add it to crontab, create script and type below code.

{% code title="backup.sh" overflow="wrap" %}
```sh
!#/bin/bash
/usr/bin/rsync -av -e ssh /backupsource/ root@destinationip:/backupdestination/
```
{% endcode %}

I have placed above script in /root/backup.sh, you can make it executable using&#x20;

```sh
chmod +x /root/backup.sh
```

now create ssh key using this command from source node.&#x20;

```sh
ssh-keygen
```

once it is created, use this command to push key to destination node.

{% code overflow="wrap" %}
```sh
scp -r /root/.ssh/id_rsa.pub root@destinationip:/root/.ssh/authorized_keys
```
{% endcode %}

for security in type these commands in destination node.&#x20;

{% code lineNumbers="true" %}
```sh
mkdir /root/.ssh 
chmod 700 /root/.ssh/ 
chmod 600 /root/.ssh/authorized_keys 
```
{% endcode %}

now edit crontab in first node

```sh
crontab -e
```

this is open crontab in default editor, eg nano or vi. now type the&#x20;

```sh
5 * * * * bash /root/backup.sh

```

{% hint style="info" %}
{% code overflow="wrap" %}
```
minute hour day week dayofweek, in crontab, 1 digit means minite, second is hour, third, is week and 5th is day of week. 
```
{% endcode %}
{% endhint %}

by now it will start working, if you want to see the cron logs, you can check using&#x20;

```sh
tail -f /var/log/cron
```
