# Wordpress Security

## Disable Directory Listing

Most web servers allow any user to browse the directories (folders) when no index file is available. This can lead to information leakage and help an attacker when trying to compromise your site.

In order to improve your security, you should disable this option. The [NIST Guide for Securing Web Servers](http://csrc.nist.gov/publications/nistpubs/800-44-ver2/SP800-44v2.pdf) also recommends it.

#### Disabling directory browsing on Apache <a href="#disabling-directory-browsing-on-apache" id="disabling-directory-browsing-on-apache"></a>

To disable directory listing on Apache, add the following line to your `.htaccess` file:



```

# Disabling directory browsing on Apache
Options -Indexes

```

