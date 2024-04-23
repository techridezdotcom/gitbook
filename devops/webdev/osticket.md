# OsTicket

## How to rest lost admin password using mysql terminal

First you need acces to VPS&#x20;

Then login to _**mysql**_ using&#x20;

```
mysql -u root -p
```

then access your _**osticket database !**_

```
mysql> show databases;
+------------------------+
| Database               |
+------------------------+
| information_schema     |    |
| mysql                  |
| osticket.techridez.com |
| page.techridez.com     |
| performance_schema     |   |
| phpmyadmin             | |
| sys                    |
| vanilla_db             |
+------------------------+
8 rows in set (0.02 sec)

mysql> use osticket.techridez.com
```

now you have to change database to your's i am using _**osticket.techridez.com**_

then use&#x20;

> ``UPDATE `ost_staff` SET `passwd` = MD5( 'password' ) WHERE `staff_id`='1';``

{% hint style="info" %}
Where ost\_staff is your _**table**_

password = you need to change with _**new password**_

staff\_id=1 = _**1**_ use used as _**admin**_
{% endhint %}

```
UPDATE `ost_staff` SET `passwd` = MD5( 'password' ) WHERE `staff_id`='1'; 
```
