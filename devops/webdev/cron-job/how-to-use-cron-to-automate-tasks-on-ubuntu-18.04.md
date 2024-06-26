# How To Use Cron to Automate Tasks on Ubuntu 18.04

#### Introduction <a href="#introduction" id="introduction"></a>

[Cron](https://en.wikipedia.org/wiki/Cron) is a time-based job scheduling daemon found in Unix-like operating systems, including Linux distributions. Cron runs in the background and tasks scheduled with cron, referred to as “cron jobs,” are executed automatically, making cron useful for automating maintenance-related tasks.

This guide provides an overview of how to schedule tasks using cron’s special syntax. It also goes over a few shortcuts one can use to make job schedules easier to write and understand.

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

To complete this guide, you’ll need access to a computer running Ubuntu 18.04. This could be your local machine, a virtual machine, or a virtual private server.

Regardless of what kind of computer you use to follow this guide, it should have a non-root user with administrative privileges configured. To set this up, follow our [Initial Server Setup guide for Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-18-04).

### Installing Cron <a href="#installing-cron" id="installing-cron"></a>

Almost every Linux distribution has some form of cron installed by default. However, if you’re using an Ubuntu machine on which cron isn’t installed, you can install it using APT.

Before installing cron on an Ubuntu machine, update the computer’s local package index:

```
sudo apt update
```

Then install cron with the following command:

```
sudo apt install cron
```

You’ll need to make sure it’s set to run in the background too:

```
sudo systemctl enable cron
```

```
OutputSynchronizing state of cron.service with SysV service script with /lib/systemd/systemd-sysv-install.
Executing: /lib/systemd/systemd-sysv-install enable cron
```

Following that, cron will be installed on your system and ready for you to start scheduling jobs.

### Understanding How Cron Works <a href="#understanding-how-cron-works" id="understanding-how-cron-works"></a>

Cron jobs are recorded and managed in a special file known as a `crontab`. Each user profile on the system can have their own crontab where they can schedule jobs, which is stored under `/var/spool/cron/crontabs/`.

To schedule a job, you just need to open up your crontab for editing and add a task written in the form of a _cron expression_. The syntax for cron expressions can be broken down into two elements: the schedule and the command to run.

The command can be virtually any command you would normally run on the command line. The schedule component of the syntax is broken down into 5 different fields, which are written in the following order:

| Field            | Allowed Values      |
| ---------------- | ------------------- |
| minute           | `0-59`              |
| hour             | `0-23`              |
| Day of the month | `1-31`              |
| month            | `1-12` or `JAN-DEC` |
| Day of the week  | `0-6` or `SUN-SAT`  |

Together, tasks scheduled in a crontab are structured like this:

```
minute hour day_of_month month day_of_week command_to_run
```

Here’s a functional example of a cron expression. This expression runs the command `curl http://www.google.com` every Tuesday at 5:30 PM:

```
30 17 * * 2 curl http://www.google.com
```

There are also a few special characters you can include in the schedule component of a cron expression to make scheduling easier:

* `*`: In cron expressions, an asterisk is a wildcard variable that represents “all.” Thus, a task scheduled with `* * * * * ...` will run every minute of every hour of every day of every month.
* `,`: Commas break up scheduling values to form a list. If you want to have a task run at the beginning and middle of every hour, rather than writing out two separate tasks (e.g., `0 * * * * ...` and `30 * * * * ...`), you could achieve the same functionality with one (`0,30 * * * * ...`).
* `-`: A hyphen represents a range of values in the schedule field. Instead of having 30 separate scheduled tasks for a command you want to run for the first 30 minutes of every hour (as in `0 * * * * ...`, `1 * * * * ...`, `2 * * * * ...`, and so on), you could just schedule it as `0-29 * * * * ...`.
* `/`: You can use a forward slash with an asterisk to express a step value. For example, instead of writing out eight separate separate cron tasks to run a command every three hours (as in, `0 0 * * * ...`, `0 3 * * * ...`, `0 6 * * * ...`, and so on), you could schedule it to run like this: `0 */3 * * * ...`.

**Note**: You cannot express step values arbitrarily; you can only use integers that divide evenly into the range allowed by the field in question. For instance, in the “hours” field you could only follow a forward slash with `1`, `2`, `3`, `4`, `6`, `8`, or `12`.\


Here are some more examples of how to use cron’s scheduling component:

* `* * * * *` - Run the command every minute.
* `12 * * * *` - Run the command 12 minutes after every hour.
* `0,15,30,45 * * * *` - Run the command every 15 minutes.
* `*/15 * * * *` - Run the command every 15 minutes.
* `0 4 * * *` - Run the command every day at 4:00 AM.
* `0 4 * * 2-4` - Run the command every Tuesday, Wednesday, and Thursday at 4:00 AM.
* `20,40 */8 * 7-12 *` - Run the command on the 20th and 40th minute of every 8th hour every day of the last 6 months of the year.

If you find any of this confusing or if you’d like help writing schedules for your own cron tasks, [Cronitor](https://cronitor.io/) provides a handy cron schedule expression editor named [“Crontab Guru”](https://crontab.guru/) which you can use to check whether your cron schedules are valid.

### Managing Crontabs <a href="#managing-crontabs" id="managing-crontabs"></a>

Once you’ve settled on a schedule and you know the job you want to run, you’ll need to put it somewhere your daemon will be able to read it.

As mentioned previously, a crontab is a special file that holds the schedule of jobs cron will run. However, these are not intended to be edited directly. Instead, it’s recommended that you use the `crontab` command. This allows you to edit your user profile’s crontab without changing your privileges with `sudo`. The `crontab` command will also let you know if you have syntax errors in the crontab, while editing it directly will not.

You can edit your crontab with the following command:

```
crontab -e
```

If this is the first time you’re running the `crontab` command under this user profile, it will prompt you to select a default text editor to use when editing your crontab:

```
Outputno crontab for sammy - using an empty one

Select an editor.  To change later, run 'select-editor'.
  1. /bin/nano        <---- easiest
  2. /usr/bin/vim.basic
  3. /usr/bin/vim.tiny
  4. /bin/ed

Choose 1-4 [1]: 
```

Enter the number corresponding to the editor of your choice. Alternatively, you could just press `ENTER` to accept the default choice, `nano`.

After making your selection, you’ll be taken to a new crontab containing some commented-out instructions on how to use it:

```
# Edit this file to introduce tasks to be run by cron.
# 
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
# 
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').# 
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
```

When you run `crontab -e` in the future, it will bring up your crontab in this text editor automatically. Once in the editor, you can input your schedule with each job on a new line. Otherwise, you can save and close the crontab for now (`CTRL + X`, `Y`, then `ENTER` if you selected `nano`).

&#x20;**Note**: On Linux systems, there is another crontab stored under the `/etc/` directory. This is a system-wide crontab that has an additional field for which user profile each cron job should be run under. This tutorial focuses on user-specific crontabs, but if you wanted to edit the system-wide crontab, you could do so with the following command:

```
sudo nano /etc/crontab
```

If you’d like to view the contents of your crontab, but not edit it, you can use the following command:

```
crontab -l
```

You can erase your crontab with the following command:

**Warning**: The following command will not ask you to confirm that you want to erase your crontab. **Only run it if you are certain that you want to erase it**.\


```
crontab -r
```

This command will delete the user’s crontab immediately. However, you can include the `-i` flag to have the command prompt you to confirm that you actually want to delete the user’s crontab:

```
crontab -r -i
```

```
Outputcrontab: really delete sammy's crontab? (y/n)
```

When prompted, you must enter `y` to delete the crontab or `n` to cancel the deletion.

### Managing Cron Job Output <a href="#managing-cron-job-output" id="managing-cron-job-output"></a>

Because cron jobs are executed in the background, it isn’t always apparent that they’ve run successfully. Now that you know how to use the `crontab` command and how to schedule a cron job, you can start experimenting with some different ways of redirecting the output of cron jobs to help you track that they’ve been executed successfully.

If you have a [_mail transfer agent_](https://en.wikipedia.org/wiki/Message\_transfer\_agent) — such as [Sendmail](https://www.proofpoint.com/us/products/open-source-email-solution) — installed and properly configured on your server, you can send the output of cron tasks to the email address associated with your Linux user profile. You can also manually specify an email address by providing a `MAILTO` setting at the top of the crontab.

For example, you could add the following lines to a crontab. These include a `MAILTO` statement followed by an example email address, a `SHELL` directive that indicates the shell to run (`bash` in this example), a `HOME` directive pointing to the path in which to search for the cron binary, and a single cron task:

```
. . .

MAILTO="example@digitalocean.com"
SHELL=/bin/bash
HOME=/

* * * * * echo ‘Run this command every minute’
```

This particular job will return “Run this command every minute,” and that output will get emailed every minute to the email address specified after the `MAILTO` directive.

You can also redirect a cron task’s output into a log file or into an empty location to prevent getting an email with the output.

To append a scheduled command’s output to a log file, add `>>` to the end of the command followed by the name and location of a log file of your choosing, like this:

```
* * * * * echo ‘Run this command every minute’ >> /directory/path/file.log
```

Let’s say you want to use cron to run a script but keep it running in the background. To do so, you could redirect the script’s output to an empty location, like `/dev/null` which immediately deletes any data written to it. For example, the following cron job executes a PHP script and runs it in the background:

```
* * * * * /usr/bin/php /var/www/domain.com/backup.php > /dev/null 2>&1
```

This cron job also redirects [_standard error_](https://en.wikipedia.org/wiki/Standard\_streams#Standard\_error\_\(stderr\)) — represented by `2` — to standard output (`>&1`). Because standard output is already being redirected to `/dev/null`, this essentially allows the script to run silently. Even if the crontab contains a `MAILTO` statement, the command’s output won’t be sent to the specified email address.

### Restricting Access <a href="#restricting-access" id="restricting-access"></a>

You can manage which users are allowed to use the `crontab` command with the `cron.allow` and `cron.deny` files, both of which are stored in the `/etc/` directory. If the `cron.deny` file exists, any user listed in it will be barred from editing their crontab. If `cron.allow` exists, only users listed in it will be able to edit their crontabs. If both files exist and the same user is listed in each, the `cron.allow` file will override `cron.deny` and the user will be able to edit their crontab.

For example, to deny access to all users and then give access to the user **ishmael**, you could use the following command sequence:

```
sudo echo ALL >>/etc/cron.deny
sudo echo ishmael >>/etc/cron.allow
```

First, we lock out all users by appending `ALL` to the `cron.deny` file. Then, by appending the username to the `cron.allow` file, we give the **ishmael** user profile access to execute cron jobs.

Note that if a user has `sudo` privileges, they can edit another user’s crontab with the following command:

```
sudo crontab -u user -e
```

However, if `cron.deny` exists and user is listed in it and they aren’t listed in `cron.allow`, you’ll receive the following error after running the previous command:

```
OutputThe user user cannot use this program (crontab)
```

By default, most cron daemons will assume all users have access to cron unless either `cron.allow` or `cron.deny` exists.

### Special Syntax <a href="#special-syntax" id="special-syntax"></a>

There are also several shorthand commands you can use in your crontab file to help streamline job scheduling. They are essentially shortcuts for the equivalent numeric schedule specified:

| Shortcut   | Shorthand for |
| ---------- | ------------- |
| `@hourly`  | `0 * * * *`   |
| `@daily`   | `0 0 * * *`   |
| `@weekly`  | `0 0 * * 0`   |
| `@monthly` | `0 0 1 * *`   |
| `@yearly`  | `0 0 1 1 *`   |

**Note**: Not all cron daemons can parse this syntax (particularly older versions), so double-check it works before you rely on it.\


Additionally, the `@reboot` shorthand will run whatever command follows it any time the server starts up:

```
@reboot echo "System start up"
```

Using these shortcuts whenever possible can help make it easier to interpret the schedule of tasks in your crontab.

### Conclusion <a href="#conclusion" id="conclusion"></a>

Cron is a flexible and powerful utility that can reduce the burden of many tasks associated with system administration. When combined with shell scripts, you can automate tasks that are normally tedious or complicated. For instance, you could [write a shell script to send data backups to an object storage solution and then automate it with cron](https://www.digitalocean.com/community/tutorials/how-to-automate-backups-digitalocean-spaces).



> Got it from&#x20;
>
> [https://www.digitalocean.com/community/tutorials/how-to-use-cron-to-automate-tasks-ubuntu-1804](https://www.digitalocean.com/community/tutorials/how-to-use-cron-to-automate-tasks-ubuntu-1804)
>
>
