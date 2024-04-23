# Install ClamAV antivirus in Ubuntu Server and Client With Cron job

In this how to we will install [ClamAV](http://www.clamav.net) antivirus on an Ubuntu client and a server. First we will install it on the client with the following command:Install clamav packagesudo apt-get install clamav clamtk

```
sudo apt-get install clamav clamtk
```

This will install **ClamAV** and the GUI frontend [ClamTK](http://clamtk.sourceforge.net). You can configure daily scans and virus definition updates inside this tool.

To install ClamAV on an Ubuntu server we start by installing ClamAV and the **daemon** by executing the following commands:

```
sudo apt-get install clamav clamav-daemon
```

Next we need to reconfigure the ClamAV base package, update the virus definitions and start the daemon. Execute the following commands\


> Reconfigure ClamAV and update virus definitions

\#set the maximum directory recursion to 50 such that all directories are getting scanned

\# set to follow directory sym links to true

```
sudo dpkg-reconfigure clamav-base
sudo freshclam
sudo /etc/init.d/clamav-daemon start
```

Next we need to create a shell script which scans a specific directory and sends an email if a virus is found. Place that shell script inside the user home of the root user or somewhere else. I placed it inside _/home/clamav._ Ok now create a file with the command _**‘sudo**_ _**vi clamav-scan.sh’**_ and enter the following content:



**clamav-scan.sh#!/bin/sh**\


```
#!/bin/sh
 
# emtpy the old scanlog and do a virus scan
rm -R /home/root/clamav/clamav-scan.log
touch /home/root/clamav/clamav-scan.log
clamdscan /home/ /etc/ /opt/ --fdpass --log=/home/root/clamav/clamav-scan.log --infected --multiscan
 
### Send the email
if grep -rl 'Infected files: 0' /home/root/clamav/clamav-scan.log
then echo "No virus found inside /home."
else cat /home/root/clamav/clamav-scan.log | mail -s "Virus warning inside folder /home" root
fi
```

Next we need to make the file executable with the following command:

> Make the clamav-scan.sh executablesudo

```
chmod +x clamav-scan.sh
```

After that we add this file as a cronjob which executes every night at 3am:

> Add the cronjob for the scan

```
sudo crontab -e
```

{% hint style="info" %}
\# enter the following line\
00 03 \* \* \* {PATH-TO-SCRIPT}/clamav-scan.sh
{% endhint %}

Substitute the _{PATH-TO-SCRIPT}_ placeholder with the path where the **clamav-scan.sh** script is stored.

Next we infect the folder you want to scan with the EICAR test virus. For that create a text file and add the following content to it:

> EICAR test virus

```
X5O!P%@AP[4\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*
```

Store it and then run the created **clamav-scan.sh** to see if the virus is found and the mail is sent. After everything worked as it should, delete the test virus text file.

**But be aware:** The clamav-scan.sh script identifies the viruses and doesn’t delete them, that has to be done manually.



{% hint style="info" %}
Source [https://guylabs.ch/2013/09/18/install-clamav-antivirus-in-ubuntu-server-and-client/](https://guylabs.ch/2013/09/18/install-clamav-antivirus-in-ubuntu-server-and-client/)
{% endhint %}
