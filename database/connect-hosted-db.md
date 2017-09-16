## Connecting with a hosted database using composer

### Install composer globally
I found good instructions for the initial install here:
https://getcomposer.org/download/

### Install composer locally
a) `composer install`

b) if using a hosted database, provide the url when prompted  

c) hit [enter] for the rest of the option as the default settings are fine  
When I finished this, I got two runtime exceptions:  
`Unable to create the cache directory (/home/lucy/Documents/Outlandish/nhscuts/var/cache/dev)`  

d) I cleared the cache  
`var/cache`  
`ls` // this is to see what's in there  
`sudo rm -r [whatever-is-in-there]`  
enter password when prompted  
return to root Directory  
run `ls -la ./var/cache` to check the permissions settings  
then run `sudo chown -R lucy:www-data ./var/cache`  
and `sudo chmod -R 0775 ./var/cache`  
then run `composer install` again to check if the runtime exceptions are gone  

e) Another thing that could happen is that you could get a 404 back from the database:  
```
Possibly unhandled rejection: {"data":"<!DOCTYPE HTML PUBLIC \"-//IETF//DTD HTML 2.0//EN\">\n<html><head>\n<title>404   Not Found</title>\n</head><body>\n<h1>Not Found</h1>\n<p>The requested URL/_Outlandish/nhscuts/web/api/   query-service-name was not found on this server.</p>\n<hr>\n<address>Apache/2.4.18 (Ubuntu) Server at localhost Port   80</address>\n</body></html>\n","status":404,"config":{"method":"GET","transformRequest":[null],"transformResponse":[null],  "jsonpCallbackParam":"callback","url":"api/query-service-name?search=islington","headers":{"Accept":"application/json,   text/plain, */*"}},"statusText":"Not Found"}
```
In this case, run `sudo chmod -R 0777 <project_root>/var`  
