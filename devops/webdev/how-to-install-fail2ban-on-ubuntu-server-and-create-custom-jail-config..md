# How to install fail2ban on ubuntu server and create custom jail config.

```
sudo apt install fail2ban
```

```
sudo systemctl enable fail2ban
```

```
sudo systemctl start fail2ban
```

to enable custom config use this, for this tutorial i am using sshd service&#x20;

i am creating custom jail config file. for that go to /etc/fail2ban directory then.

create jail.local

```
[sshd]
enabled = true
port = ssh
filter = sshd
logpath = /var/log/auth.log
maxretry = 2
bantime = -1
```

and save this.&#x20;

```
systemctl restart fail2ban.service
```

restart fail2ban.service, after some time if you check the status using sshd jail name you can see the status.&#x20;

```
root@techridez:~# fail2ban-client status sshd
Status for the jail: sshd
|- Filter
|  |- Currently failed: 0
|  |- Total failed:     0
|  `- File list:        /var/log/auth.log
`- Actions
   |- Currently banned: 93
   |- Total banned:     93
   `- Banned IP list:   107.189.11.160 112.70.191.130 116.106.16.243 117.121.214.50 125.167.94.54 126.0.63.53 141.98.10.195 141.98.10.197 141.98.10.200 141.98.10.209 141.98.10.211 141.98.10.214 141.98.9.137 141.98.9.157 141.98.9.160 141.98.9.161 141.98.9.162 141.98.9.163 141.98.9.165 141.98.9.166 142.217.140.186 144.217.12.194 155.4.49.126 162.247.74.27 167.71.187.10 170.253.25.93 171.226.1.221 171.226.5.194 171.227.209.13 171.227.215.169 171.232.247.194 171.235.81.151 171.235.84.220 171.249.138.32 174.127.236.143 176.118.44.112 178.165.72.177 178.32.124.74 178.38.201.149 180.126.224.93 182.211.245.169 185.10.68.22 185.114.80.208 185.220.102.248 185.220.102.250 185.54.155.200 192.42.116.16 193.228.91.108 194.180.224.130 194.61.26.117 194.61.26.211 198.251.89.157 198.251.89.80 198.98.49.181 2.226.157.66 20.187.47.39 211.250.72.142 218.75.156.247 220.134.25.185 221.149.43.38 221.163.31.174 222.180.149.101 23.129.64.209 31.17.18.140 31.173.168.226 42.248.93.10 45.227.255.4 49.83.146.216 50.66.167.29 51.77.135.89 51.79.86.173 52.139.249.186 52.183.97.14 62.102.148.69 62.210.37.82 62.98.67.163 67.10.248.60 68.5.173.208 74.129.23.72 74.97.19.201 76.67.192.249 78.56.227.1 80.234.165.143 82.243.97.188 83.59.35.39 86.86.41.22 90.186.220.205 90.3.72.252 91.224.22.76 93.208.252.234 95.109.94.168 95.70.148.19 98.15.156.16
```
