## The process object

This week I got a bit friendlier with the process object. Not because I wanted to but because I accidentally overwrote it.

I was working on one of my old projects (node.js, hapi, handlebars) that I threw together during a coding bootcamp. It's a weather app that gets the user's ip address and tells them the weather, but translated into pirate speak (yes, it's trivial). I built it to get a better undertanding of clientside api calls. Later I refactored the project to make the calls from the server so that I could save the api keys as environment variables and be able to put the project on heroku.

The problem with my initial refactoring was that the ip picked up was that of the server (it turns out it's in Dublin) so the weather that was served up was for Dublin, not the location of the user. So the refactoring I was doing was to add a clientside js file to get the user's location and send it to the server, which could then make the calls to the weather and pirate-speak apis and serve up the page with the weather to the user.

It was late and I was trying to refactor my api calls into cleaner code with better separation of concerns. As I worked, I noticed that, all of sudden, I was getting an error relating to the environment variables, which was extremely puzzling. I didn't record the error but the gist of it was that the weather api key, which I had saved in a .env file was coming back as undefined.

As I had installed an npm module earlier that evening that I didn't end up using, and I installed it around the time that the environment variable disconnect happened, so I uninstalled it. Then I checked in with env2, which is the npm module I use for my .env file. I tried changing the name of the file and double checked the syntax, but none of that made any difference.

As a sanity check, I then tried out the full url in the browser, which was fine (predictably). Then I plugged the api key right into the url (in the js file) and that worked fine too. So the problem was somewhere in the connection with the process object.

The property of the process object that was relevant is .env, so I took a look at [the node.js documentation for `process.env`](https://nodejs.org/api/process.html#process_process_env). While interesting, that didn't provide me with much traction, so I took a look at the description of the object itself:
>The process object is a global that provides information about, and control over, the current Node.js process. As a global, it is always available to Node.js applications without using require().

The answer was actually available in the quoted paragraph, but I didn't make the connection.

I was tired, so I gave up for the night and shut down. In the morning, when I started (paid-for) work, I found that I was not able to connect with the aws-hosted elasticsearch database. I re-ran the setup command for elastic search and got the following error message: `Error: Unable to reach Elasticsearch Server! Check that service is running.`

I wondered if there was a connection between the problem I had with my side-project and this issue as in both cases, the connections relied on environment variables stored in .env files. It was at this point that I became concerned that I had broken something significant.

I asked the tech lead on my project if there was any reason I couldn't connect with elasticsearch, and he asked for my ip address. I didn't ask why, but I did some digging afterwards and I believe it's for [the reasons described in this blog post](https://www.elastic.co/blog/elasticsearch-unplugged). He plugged in my ipv6 and then ipv4. Still nothing.

Next, I got in touch with another developer, who has often helped me out when I'm in a bind. Knowing there are people like that who are there for you makes a huge difference to your morale. I think it's worth noting the process that he took in helping me diagnose the problem.

1. He asked me to describe the problem.
2. He asked me to push the code up to github so he could replicate the error
3. He replicated the error (and I felt weirdly relieved that I hadn't just done something bone-headed)
4. He confirmed that the problem was indeed associated with the process object
5. He suggested that I log the process object in the index.js file (a file at the root with server.start() in it)

From index.js I was able to confirm that the process object was absolutely fine when the server started. Then I started lacing the files with console logs. The process object was still working in the route that called a function that made a request to the weather api. However, inside the function itself, it was broken.

I replaced the api key parameter with the actual key and found that all of a sudden I was getting a different error, one that said that a different property of process was missing, and the error was located further down in the file. It was at that point as my gaze moved through the file and I realised that I was exporting a function called 'process' on the module object. The previous evening, I had refactored this function to be a method on module.exports (`module.exports.process = function() {...}`). So this was the problem: all I needed to do to solve it was to rename the method.

Before I refactored, I had an object in the file called 'weather', and 'process' was a method on that object. My understanding is that when I attached it to module.exports, the barrier between the namespace that my weather object provided suddenly disappeared, and the process method got the same scope as the (global) process object. My lack of understanding led me to fear that I had unwittingly broken the process object with wider consequences. As it turned out, this fix solved the issue with this project but did nothing to resolve the problem with elasticsearch. This is because the process object relates to the current node.js process.

So, in looking back at the node documentation the information I needed was there (my emphasis):
>The process object is a **global** that provides information about, and control over, **the current Node.js process**. As a global, it is always available to Node.js applications without using require().

My takeaways from this:
1. This is an example of why global scope can be the third rail
2. Namespace can be a minefield in js, which will merrily overwrite important stuff without checking 'Are you sure you want to overwrite the process object? Really? Really really?'
3. Don't ever name anything 'process', ever again
