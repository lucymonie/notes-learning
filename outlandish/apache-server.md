## Apache server: setting up multiple sites on localhost

I already had an apache server set up, but I wanted to use the one server for multiple sites. To do this, you can include a config file for each site in the enabled-site folder in `/etc/apache2/sites-enabled/`

#### Instructions:

1. Set up a new config file  
a) cd /etc/apache2/sites-enabled  
b) the file should be called something relating to the project, and the file name must end in .conf  
c) need to sudo into atom to make the changes and save them  

2. Set up SymLinks  
a) `cd /var/www`  
b) `ls` to see what's in there already  
c) `sudo ln -s /home/lucy/Documents/Outlandish/[project-folder] [alias-for-project]`  
d) `ls -l` to see that it's there (in detail)  
e) add the following to the file  

```
    ServerName [project-name].lvh.me

    ServerAdmin webmaster@localhost
      DocumentRoot /var/www/[alias-for-project]/web
     <Directory /var/www/[alias-for-project]>
      Options FollowSymLinks
     </Directory>
     <Directory /var/www/[alias-for-project]/web>
          AllowOverride All
    </Directory>
```

3. Restart apache server  
`sudo service apache2 restart`  

5. Try running the server at the url specified in the file in sites-enabled (ServerName)  
If it's fine, all good. Otherwise, go on:  

6. If need be, remove restrictions on folders  
a) read errors: `cd /var/log/apache2`, then `tail error.log`  
b) `cd /home/lucy/Documents/Outlandish/[project-folder]`  
c) Modify the permissions for var: `chmod a+w var`  
d) `cd var`  
e) Modify the permissions for logs and sessions:  
    `chmod a+w logs`  
    `chmod a+w sessions`  
