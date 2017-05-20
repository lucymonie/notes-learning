## MySQL connection with PHP
*Notes by Joaquim d'Souza*   

Make sure PHP can access your MySQL database. For now, let PHP log in as 'root', but this is pretty bad security wise so we'll have to return to it at some point. Ideally you'd create a user for each website you make, and only allow it access to the data it needs.  

The root account may or may not have a password. Check with these commands:  
```
- mysql -u root          # connect to mysql as root with no password  
- mysql -u root -p       # connect to mysql as root with a password prompt  
```
If the first command works, you've got no password on your 'root' account. If it doesn't, then the root account has a password (which hopefully you know! if you don't, you'll have to reset it (google 'reset mysql root password ubuntu'))  

This seems like a good PHP MySQL tutorial: http://www.tutorialrepublic.com/php-tutorial/php-mysql-introduction.php. It mentions http://localhost/phpmyadmin/, but you won't have that installed so just ignore that part. I think the first few parts are good:  

PHP MySQL Introduction  
PHP MySQL Connect  
PHP MySQL Create Database  
PHP MySQL Create Table  
PHP MySQL Insert  
PHP MySQL Prepared  

But don't worry about going beyond that.  

3) Install phpMyAdmin – a PHP web application for administrating your MySQL database:  

`sudo apt install phpmyadmin`  

This should automatically put a /phpmyadmin/ folder in apache's document root. Check by visiting http://localhost/phpmyadmin.  
If it doesn't work I'm afraid I'm not sure how to debug, there are a lot of tutorials on how to do it online but they are all slightly different. Fundamentally the phpmyadmin folder (or a "symbolic link" to it) must be in your document root, and if it isn't, it won't work.  

Then just visit http://localhost/phpmyadmin and enter your MySQL username ('root') and password.  

4) Install WordPress! https://codex.wordpress.org/Installing_WordPress

Good luck! Let me know how you get on. I'm not sure how long all this stuff would take to get through as you might hit a lot of blockers, e.g.:

- general PHP weirdness might be confusing when going through the tutorials
- logging for debug purposes is difficult... PHP can log to a file, but it is not convenient. Check http://php.net/manual/en/function.error-log.php, and the comments
- setting up mysql – specifically being able to log in as 'root' – might be tricky
- php not being able to connect to mysql
- the phpmyadmin application might not work and be difficult to debug
- logging in to phpmyadmin might not work
- sometimes PHP requires you to add stuff to the configuration file (check http://localhost/phpinfo.php for the config file location)
- installing wordpress sometimes goes wrong

Some of those I'll know how to help you with, and some of them I won't. The first step will almost always be to check the error log, which is probably in `/var/log` somewhere (because of the [file system hierarchy standard]( https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard)) With a bit of linux experience you'll be able to guess where most files are going to be found. Check [ask ubuntu](http://askubuntu.com/questions/14763/where-are-the-apache-and-php-log-files) for more help.
