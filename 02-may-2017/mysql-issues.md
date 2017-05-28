## Issues with MySQL

One of the things that I struggle with as a junior developer is my dependency on others. Like most people I enjoy feeling competent, and I find it hard to be turning to the person next to me and asking for help on a frequent basis, especially when I know they're likely in the middle of something fairly complicated. However, I also know that it's not cool to waste my employer's time when I'm truly stuck, so I ask.

This week I was joined a team on my first wordpress project, which included a MySQL database. I installed MySQL when I did my LAMP installation, so I figured it should be straightforward.

I turned to Digital Ocean, which (of course) had some nice simple instructions for setting up a database, but I had some trouble. First, it turned out that I had no password. So I dutifully googled 'reset password mysql database', and I found some good instructions for that [here](https://help.ubuntu.com/community/MysqlPasswordReset) (suitable for Ubuntu 16.04).

I got to this command: `sudo /usr/sbin/mysqld --skip-grant-tables --skip-networking &` and then I ran into an error:
`ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/run/mysqld/mysqld.sock' (2)`

I found [a post on Stack Overflow](https://stackoverflow.com/questions/11657829/error-2002-hy000-cant-connect-to-local-mysql-server-through-socket-var-run) that seemed to related the exact same issue.

I renamed the file at `/etc/mysql/my.cnf` and hey presto I was in. I changed the root password, and then I tried to exit, and the same error came up. Evidently I hadn't solved the core issue. I asked for help, which had to be escalated to the most senior developer in the building. It was interesting to watch his diagnostic process. At the end of 20 minutes, he advised me to reinstall MySQL.

- I uninstalled using [these instructions on AskUbuntu]( https://askubuntu.com/questions/640899/how-do-i-uninstall-mysql-completely)
- I reinstalled using [these instructions on Digital Ocean](https://www.digitalocean.com/community/tutorials/how-to-install-the-latest-mysql-on-ubuntu-16-04
)  

Interestingly, as I was following the instructions in the reinstallation, I recalled that when I did the inital install things must have gone awry as I wasn't prompted to set a password - something I had forgotten until I went through the process again, and was prompted.    

So finally I was in! I created a database in a matter of seconds, using [Digital Ocean](https://www.digitalocean.com/community/tutorials/how-to-create-and-manage-databases-in-mysql-and-mariadb-on-a-cloud-server) and then I was in business.

I needed to install the data, which is did using [a terminal command from Stack Overflow](https://stackoverflow.com/questions/17666249/how-to-import-an-sql-file-using-the-command-line-in-mysql): `mysql -u username -p database_name < file.sql` and this worked smoothly.

So my learning from this crazily convoluted process is this: if things go off piste when you're installing a critical piece of software, something is probably wrong. Investigate, don't just shrug!

Also, another developer suggested installing MySQL Workbench, [instructions here](https://dev.mysql.com/doc/workbench/en/wb-installing-linux.html), which makes it easier to trouble-shoot MySQL.
