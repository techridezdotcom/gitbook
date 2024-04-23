# Apache Virtual Host Script with mysql db.

lamp-vhost-manager usage

first install lamp server

{% embed url="https://github.com/Marko-M/lamp-vhost-manager" %}

Download script and make file executable using

`chmod +x lamp-vhost-manager.sh`

then&#x20;

./lamp-vhost-manager.sh -m add -n support.example.com -u root -p password

{% hint style="info" %}
Where root is your database name

support.example.com is your url

password is your password&#x20;

you have to change it to your need
{% endhint %}

&#x20;`cd  /etc/apache2/sites-available`

&#x20;mv support.example.com support.example.com.conf&#x20;

a2ensite support.example.com&#x20;

`systemctl reload apache2`

