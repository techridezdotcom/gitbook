# How to Backup MySQL Databases Automatically on Ubuntu

### Not backing up your customer’s data regularly is detrimental to the trustworthiness of your service. This tutorial shows how to back up MySQL data at regular intervals. <a href="#id-8dc5" id="id-8dc5"></a>

Regular backups are important with any kind of data, and this is especially relevant when talking about the data of your customers who trust to have their data available anytime.

This tutorial introduces an excellent tool that helps you backup MySQL databases at regular intervals (i.e. daily, weekly, and monthly), is completely automatic and easy to setup.

## Using mysqldump and Restore <a href="#fc92" id="fc92"></a>

The conventional (and more tedious) way is to dump your database’s data into an SQL file. The basic syntax of the command is:

```
$ mysqldump -u username -p database_name > backup.sql
```

To restore from an SQL file, use this command:

```
$ mysql -u username -p database_name < backup.sql
```

You could spin this further and create a cron job which automatically assigns a new file name containing the timestamp to ensure that previous backups are not replaced by a new one. Luckily, there is a tool _automysqlbackup_ which does everything for you.

## How to Backup MySQL using automysqlbackup <a href="#id-3cfe" id="id-3cfe"></a>

_automysqlbackup_ is a utility program that is available in the Ubuntu repositories. This utility can be scheduled to automatically perform backups at regular intervals. To install, type the following into the terminal:

```
$ sudo apt-get install automysqlbackup
```

A following prompt will ask you which mail configuration you prefer. Select “internet site” if you’re going to set up email notification. If not, just select “no configuration”. I personally went for “no configuration” to leave things unchanged. Start _automysqlbackup_:

```
$ sudo automysqlbackup
```

A folder structure and initial backups will be performed immediately. The default location for backups is `/var/lib/automysqlbackup`. List contents of this directory to see the folder structure:

```
$ ls /var/lib/automysqlbackup
```

You should see three directories for the `daily`, `weekly` and `monthly` backups being performed automatically by _automysqlbackup_ and a subdirectory for each database containing the respective gzipped SQL dump.

The configuration file for _automysqlbackup_ is located at `/etc/default/automysqlbackup`. Open it in your favorite editor:

```
sudo nano /etc/default/automysqlbackup
```

This file assigns many variables by the MySQL file located at `/etc/mysql/debian.cnf`. It reads the user, password, and databases to be backed up. All default configurations can be left intact. You are now able to sleep well at night, knowing that your customer’s data will be backed up at regular intervals.

> Got it from&#x20;

[https://medium.com/@mhagemann/how-to-backup-mysql-databases-automatically-on-ubuntu-17-10-a2b29fb47ac9](https://medium.com/@mhagemann/how-to-backup-mysql-databases-automatically-on-ubuntu-17-10-a2b29fb47ac9)

Thanks for the info.
