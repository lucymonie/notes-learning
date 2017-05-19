## Problems using chart.js with gulp

I was using chart.js in a project, and for some reason the data points weren't showing up in the charts. There was an error appearing in the console:   
`Uncaught ReferenceError: o is not defined`  

The error related to a file called `vendor.min.js`  

The fix involved a change to the gulpfile: from `'/chart.js/dist/Chart.min.js',` to `'/chart.js/dist/Chart.js',`  

By taking out `.min` it de-minified the file, which seemed to allow the data points to be displayed.  

The advice I got on how to solve these sorts of issues (thanks Joaquim!) is as follows:  

1) use the Chrome dev tools to see the line of code that was erroring  
2) notice that I can't read that code because it's minified  
3) disable the minifier - it works!  
4) narrow down which script is causing the minifier to fail  
5) replace this script with a non-minified version  

A vendor script not working is quite rare. But sometimes you will have to debug it, and this is impossible when the code is minified.
