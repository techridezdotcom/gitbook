# 404 hyperlink not working after wordpress migration

![Issue will look likes this ](<../../.gitbook/assets/hyperlink now working.PNG>)

> Make sure you change Hyperlink settings from old wordpress to new one exactly the same.

log into your vps

using console to to&#x20;

/etc/apache2/sites-available

then open your configuration file&#x20;

then change AllowOverride none to&#x20;

AllowOverride All

save it&#x20;

then&#x20;

```
a2enmod rewrite
```

then

```
systemctl restart apache2
```

or&#x20;

```
systemctl restart apache2.service
```



