# Auto-Restart MySQL When It Crashes During a Brute Force Attack

During a brute force attempt, running on a low-memory virtual private server (VPS) can cause MySQL to crash and render your websites unusable.

Especially WordPress sites are targeted because they run on PHP and MySQL and are apparently powering over [28% of the internet’s content management systems](https://w3techs.com).

The most fruitful solution is to permanently increase your VPS’s memory, but I can understand that administrators refuse to pay for resources that are otherwise never utilized during normal operation. To keep things up and running automatically after a crash, I use a simple cronjob to check and restart MySQL if it is down.

Load the crontab editor in the terminal with `crontab -e` and add the following line:



```
* * * * * service mysql status > /dev/null || service mysql start
```



This checks if MySQL is running every minute and redirects stdout to null.

Starting the service will not output anything unless something goes wrong, so there is no need to add the null redirect on the last command. The double pipe `||` means `OR` and will execute the second command only if the first command fails. In other words: if MySQL’s status returns an exit code greater than zero (first command), start MySQL (second command).

This is unlike the double ampersand `&&` which is similar to saying, “Run the first command, **and**, only if the first command was successful, run the second command.”

If checking every single minute is too frequent, a five-minute interval will do just as fine for smaller websites:

```
*/5 * * * * service mysql status > /dev/null || service mysql start
```

{% hint style="info" %}
Got it from&#x20;

[https://medium.com/@mhagemann/how-to-auto-restart-mysql-when-it-crashes-during-a-brute-force-attack-d7a03b726b7e](https://medium.com/@mhagemann/how-to-auto-restart-mysql-when-it-crashes-during-a-brute-force-attack-d7a03b726b7e)

Thanks for the article.
{% endhint %}
