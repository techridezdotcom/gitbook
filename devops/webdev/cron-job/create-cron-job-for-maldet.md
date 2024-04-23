# Create cron job for maldet scanning

first you need to edit maldet monitor path

for that to to /usr/local/maldetect/monitor\_paths

```
#add your paths for scanning 
/home
/etc
/var/www/
```

then save that&#x20;

go to home

I am using root so my directory is /root

then create maldet.sh and edit&#x20;

```
#start maldet service
systemctl start maldet.service
#start maldet monitor
maldet --monitor /usr/local/maldetect/monitor_paths

```

above command will enable maldet service and run monitor to the paths which is already mentioned&#x20;

now edit cron job

```
sudo crontab -e
```

if you are using first time it will ask for your edit which you want to access cron job, i am using nano, which is easier.

{% hint style="info" %}
if you messed up by selecting wrong editor ?&#x20;

No problem, use this link to refix.

[https://docs.techridez.com/techridez/devops/webdev/cron-job/how-to-change-default-crontab-editor](https://docs.techridez.com/techridez/devops/webdev/cron-job/how-to-change-default-crontab-editor)
{% endhint %}

```
# Edit this file to introduce tasks to be run by cron.
#
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
#
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').
#
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
#
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
#
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
#
# For more information see the manual pages of crontab(5) and cron(8)
#
# m h  dom mon dow   command
00 23 * * * /root/maldet.sh

```

as you can see only one command is there which is uncommented, every day at 23 Hours this will triger maldet scanning.&#x20;

enjoy.
