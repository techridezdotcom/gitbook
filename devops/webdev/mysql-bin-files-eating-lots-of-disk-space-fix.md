# MySQL Bin Files Eating Lots of Disk Space (fix)

some time you may find mysql failed to start, and if you check status of mysql you can find it will recommend you to use journalctl -xe. from that you can find mysql is out of space&#x20;

```bash
du -sh /var/lib/mysql
```

use above command and you can find size of this folder, probably this is where your server consume most of the disk space.

there are 3 methods to fix this.&#x20;

## Method #1 Manually Delete log files.

you need to change directory to /var/lib/mysql, if you type ls you can find your logs some thing like this&#x20;

```bash
48M     /var/lib/mysql/binlog.000141
102M    /var/lib/mysql/binlog.000142
67M     /var/lib/mysql/binlog.000143
104M    /var/lib/mysql/binlog.000144
102M    /var/lib/mysql/binlog.000145
101M    /var/lib/mysql/binlog.000146
103M    /var/lib/mysql/binlog.000147
44M     /var/lib/mysql/binlog.000148
```

you can also find bindlog.index file this contains log files name, you be carefull to do some thing stupid, you need take a backup first, then edit this file and remove indexing "remove names of logs" then save this.&#x20;

after that you can delete files which is not in the index

now you can restart msyql it will start,&#x20;

## Method #2 using mysql

if you mysql is still working you need to access mysql then type following command

```bash
mysql> SHOW BINARY LOGS;
+---------------+-----------+-----------+
| Log_name      | File_size | Encrypted |
+---------------+-----------+-----------+
| binlog.000141 |  50260145 | No        |
| binlog.000142 | 106706425 | No        |
| binlog.000143 |  69240464 | No        |
| binlog.000144 | 108516594 | No        |
| binlog.000145 | 106324989 | No        |
| binlog.000146 | 105725450 | No        |
| binlog.000147 | 107466759 | No        |
| binlog.000148 |  98082094 | No        |
+---------------+-----------+-----------+
```

##

this will list your binary logs index, to delete logs to a certain number, i am going to delete **binlog.000141** so i need to type command like this, the number mentioned below is the number of log which should apper in index file after removing, so in our case **141** will be deteted or all logs from **1** to **141** and rest of the logs will remain.&#x20;

```bash
PURGE BINARY LOGS TO 'binlog.000142';
```

i have put mysql logs for comparison before and after doing the purge.&#x20;

```bash
mysql> SHOW BINARY LOGS;
+---------------+-----------+-----------+
| Log_name      | File_size | Encrypted |
+---------------+-----------+-----------+
| binlog.000141 |  50260145 | No        |
| binlog.000142 | 106706425 | No        |
| binlog.000143 |  69240464 | No        |
| binlog.000144 | 108516594 | No        |
| binlog.000145 | 106324989 | No        |
| binlog.000146 | 105725450 | No        |
| binlog.000147 | 107466759 | No        |
| binlog.000148 |  98082094 | No        |
+---------------+-----------+-----------+
8 rows in set (0.01 sec)

mysql> PURGE BINARY LOGS TO 'binlog.000142';
Query OK, 0 rows affected (0.01 sec)

mysql> SHOW BINARY LOGS;
+---------------+-----------+-----------+
| Log_name      | File_size | Encrypted |
+---------------+-----------+-----------+
| binlog.000142 | 106706425 | No        |
| binlog.000143 |  69240464 | No        |
| binlog.000144 | 108516594 | No        |
| binlog.000145 | 106324989 | No        |
| binlog.000146 | 105725450 | No        |
| binlog.000147 | 107466759 | No        |
| binlog.000148 |  98195420 | No        |
+---------------+-----------+-----------+
7 rows in set (0.00 sec)

```

## Method #3 set persist binlog settings.

{% hint style="info" %}
Now we can set automatically delete old log files!

In MySQL 8.0, use binlog\_expire\_logs\_seconds instead, where the default value is 2592000 seconds (30 days). In this example, we reduce it to only 3 days (60 seconds x 60 minutes x 24 hours x 3 days):
{% endhint %}

```bash
mysql> SET GLOBAL binlog_expire_logs_seconds = (60*60*24*3);
Query OK, 0 rows affected (0.00 sec)

mysql> SET PERSIST binlog_expire_logs_seconds = (60*60*24*3);
Query OK, 0 rows affected (0.01 sec)
```

{% hint style="info" %}
SET PERSIST will make sure the configuration is loaded in the next restart. Configuration set by this command is stored inside /var/lib/mysql/mysqld-auto.cnf.
{% endhint %}
