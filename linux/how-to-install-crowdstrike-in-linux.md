# How to install CrowdStrike in Linux

<figure><img src="../.gitbook/assets/CrowdStrike_Logo_Red.jpg" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
Please download and upload your package, since I am using oracle Linux I am using <mark style="color:red;">falcon-sensor-x86\_64.rpm</mark>, and run following command.
{% endhint %}

```bash
yum install falcon-sensor-7.11.0-16404.el9.x86_64.rpm
```

{% code title="Terminal Output" overflow="wrap" lineNumbers="true" fullWidth="true" %}
```bash
Last metadata expiration check: 2:25:12 ago on Fri 12 Apr 2024 09:00:38 AM EDT.
Dependencies resolved.
====================================================================================================================
 Package                     Architecture         Version                          Repository                  Size
====================================================================================================================
Installing:
 falcon-sensor               x86_64               7.11.0-16404.el9                 @commandline                56 M

Transaction Summary
====================================================================================================================
Install  1 Package

Total size: 56 M
Installed size: 68 M
Is this ok [y/N]: y
Downloading Packages:
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                            1/1
  Running scriptlet: falcon-sensor-7.11.0-16404.el9.x86_64                                                      1/1
  Installing       : falcon-sensor-7.11.0-16404.el9.x86_64                                                      1/1
  Running scriptlet: falcon-sensor-7.11.0-16404.el9.x86_64                                                      1/1
Created symlink /etc/systemd/system/multi-user.target.wants/falcon-sensor.service → /usr/lib/systemd/system/falcon-sensor.service.

  Verifying        : falcon-sensor-7.11.0-16404.el9.x86_64                                                      1/1

Installed:
  falcon-sensor-7.11.0-16404.el9.x86_64

Complete!

```
{% endcode %}

{% hint style="warning" %}
to activate you need cid, please access your CrowdStrike console and find your CID, and run the following command with your CID.
{% endhint %}

```bash
/opt/CrowdStrike/falconctl -s --cid=F9B68DFAEWC3846978EBSDBDD27C4E0BDB-CD
```

{% hint style="info" %}
{% code overflow="wrap" %}
```
run this command to enable sensor, which means autostart sensor on every boot
```
{% endcode %}
{% endhint %}

```bash
systemctl enable falcon-sensor
```

{% hint style="danger" %}
run this command to start the sensor
{% endhint %}

```
systemctl start falcon-sensor
```

{% hint style="success" %}
run this command to find the status of sensor
{% endhint %}

```bash
systemctl status falcon-sensor
```

{% code title="Terminal Output" lineNumbers="true" fullWidth="true" %}
```bash
[root@area51~]# systemctl status falcon-sensor
● falcon-sensor.service - CrowdStrike Falcon Sensor
     Loaded: loaded (/usr/lib/systemd/system/falcon-sensor.service; enabled; preset: disabled)
     Active: active (running) since Fri 2024-04-12 11:27:05 EDT; 1min 50s ago
   Main PID: 6586 (falcond)
      Tasks: 27 (limit: 99169)
     Memory: 35.7M
        CPU: 8.580s
     CGroup: /system.slice/falcon-sensor.service
             ├─6586 /opt/CrowdStrike/falcond
             └─6587 falcon-sensor

Apr 12 11:27:21 area51@techridez.com falcon-sensor[6587]: CrowdStrike(4): ConnectWithProxy: Unable to get>
Apr 12 11:27:21 area51@techridez.com falcon-sensor[6587]: CrowdStrike(4): SslConnect: Unable to connect t>
Apr 12 11:27:21 area51@techridez.com falcon-sensor[6587]: CrowdStrike(4): trying to connect to ts01-gyr-m>
Apr 12 11:27:21 area51@techridez.com falcon-sensor[6587]: CrowdStrike(4): Connected directly to ts01-gyr->
Apr 12 11:27:21 area51@techridez.com falcon-sensor[6587]: CrowdStrike(4): ValidateCertificate: Certificat>
Apr 12 11:27:21 area51@techridez.com falcon-sensor[6587]: CrowdStrike(4): SSLSocket connected successfull>
Apr 12 11:27:21 area51@techridez.com falcon-sensor[6587]: CrowdStrike(4): sock/ssl/proxy cnctd ok. First >
Apr 12 11:27:21 area51@techridez.com falcon-sensor[6587]: CrowdStrike(4): CLOUDPROTO_ESTABLISHED. AgentId>
Apr 12 11:27:21 area51@techridez.com falcon-sensor[6587]: CrowdStrike(4): ConnectToCloud successful.
Apr 12 11:27:24 area51@techridez.com systemd[1]: /usr/lib/systemd/system/falcon-sensor.service:12: PIDFil>
```
{% endcode %}
