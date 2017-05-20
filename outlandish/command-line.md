## Find & Tail
*Notes by Joaquim d'Souza*

The first two useful command line tools I learnt were `find` and `tail`.

find example: `sudo find / -name php_error.log`

what it does: searches the entire filesystem for any files called "php_error.log"

Explanation:
- `sudo` runs the command as root, which allows you to search system folders
+ `find` is the name of the command
+ `/` means "start searching in the '/' directory", i.e. the whole filesystem
+ `-name php_error.log` means "match files with the name php_error.log".

example 2: `find ~ -name *.php`
+ search your home directory (~) for any php files

example 3: `find . -name *.ini`
+ search the current directory (.) for any ini files

example 4: `find /var/log *php*`
+ search the /var/log directory for any files that have 'php' in their name


tail example: `tail -f /var/log/apache2/php_error.log`

What it does: streams the last few lines of the file to the terminal as the file is updated

Explanation:
+ `tail` is the name of the command
+ `-f` means "keep the file open" (without this, tail will just print out the last few lines and then exit)
+ `php_error.log` is the file to view


It's worth practising editing system files with nano rather than opening them with a graphical text editor. It means that you won't have to leave the terminal to make simple edits to files, which will mean it will be more convenient to do tasks with command line utilities (like `find`, `tail`, `grep`, `cat`, `cp`, `curl`...), which in turn will make you more comfortable in a linux terminal environment, which will make it easier to do all the "ops" stuff (setting up servers and databases, deploying code, etc) that is required to get a website going.

Unless you use heroku.

...actually even if you use heroku.
