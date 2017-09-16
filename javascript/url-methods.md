# URL interface

Javascript provides an interface for dealing with parts of a web address, called URL (an object constructor).

So if you want to extract the pathname from a url, you can do so like this:

```
var url = 'http://www.mymainsite.com/path1/path2/path3/path4';

var urlObject = new URL(url);
var pathname = urlObject.pathname;

console.log(pathname); // /path1/path2/path3/path4
```

Further, this method will strip out tags from the end of the web address, as follows

```
var url = 'http://www.mymainsite.com/path1/path2/path3/path4#HeadingTag';

var pathname = new URL(url).pathname;

console.log(pathname); // /path1/path2/path3/path4
```

It will still log just the pathname, stripping off any tags from the end of the url.

The constructor can be used to get the current window's url, as follows:

`var url = new URL(window.location.href);`

To access just the href, you would use `url.href` and to get the path, `url.pathname`.
