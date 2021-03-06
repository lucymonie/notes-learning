## Apache – A Web Server
*Notes by Joaquim d'Souza*

Apache is an "old fashioned" web server. You configure it to use a specific directory (e.g. **/home/lucy/website/** or the common default **/var/www**) as the **Document Root**. It will then make the files and folders in this directory available at http://localhost.

This is originally how all websites worked – just a collection of files + folders in a directory that is read by a web server. For example you might have:
```
/var/www/
- index.html
- about.html
- contact-us.html

/var/www/things/
- cats.html
- dogs.html
```
If you visited http://localhost, you'd see the contents of **index.html**. You'd also be able to visit http://localhost/about.html, and http://localhost/things/cats.html.


## PHP – Generate dynamic HTML

PHP was invented to allow websites to be a bit more dynamic. The web server setup is almost the same – you just need to enable reading PHP files.

For example, if your **Document Root** is now:
```
/var/www
- index.php
- about.php
```
Then when you visit http://localhost/about.php, your web server would read **about.php**, process it, and respond with the result.

Historically, PHP code was embedded within HTML, so you can think of it as a templating language. Anything within the delimiters **<?php** and **?>** is interpreted as PHP code by your web server, executed, and replaced with the output of running the code. For example:  
```
index.php
==========

<html>
<head></head>
<body>
<h1>My PHP Page</h1>
<?php
    echo '<p>';
    echo 'A paragraph generated by PHP on ' . date('d-m-y');
    echo '</p>;
?>
</body>
</html>
```
Would become:
```
index.php (processed)
======================
<html>
<head></head>
<body>
<h1>My PHP Page</h1>
<p>A paragraph generated by PHP on 31-03-17</p>
</body>
</html>
```

## PHP Web Apps

This is the final piece for understanding how PHP applications, like Wordpress, work. So far, we know that:

1) PHP code needs to be read and executed by a web server  
2) Web servers map routes, like **/about.html**, to **files** in the **Document Root**

So how can Wordpress make routes that look like this: https://boomcalifornia.com/2017/03/28/what-does-it-mean-to-become-californian/, without making the corresponding directory structure?

The answer is quite simple: **the web server is configured to redirect *everything* to /index.php**. This simple trick allows the routes of a website to be controlled by PHP, instead of by Apache. The PHP code has access to the original URL, so can do something like:
```
<?php

$path = $_SERVER[REQUEST_URI]

switch($path) {
    case '/':
        printIndexPage();
        break;
    case '/about':
        printAboutPage();
        break;
    default:
        printNotFound();
}

?>
```
For Apache, the configuration that sets this up is put in a file called **.htaccess** that lives in the **Document Root**. This file basically says "redirect everything to index.php".


## Summary

1) Apache makes a folder, called the Document Root, available at http://localhost.

2) By default, requests for e.g. /about.html will be mapped to the about.html file in the Document Root.

3) Apache can be configured to process PHP files. The output is sent back to the client.

4) PHP Applications (like Wordpress) work by redirecting all requests to the same index.php file. The routing is then handled by the PHP application, instead of by Apache.
