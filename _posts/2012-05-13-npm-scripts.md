---
layout: post
title: Npm Scripts!
---
When you've got a somewhat complex Node app going you can end up with a lot of necessary batch operations you need to run before actually running your app. Some people run them manually and others actually write a small script that does this for them. Either way it's a little disconnected from actually running the app.

It turns out <a href="http://npmjs.org">NPM</a> actually has an amazing solution for this. NPM supports a <a href="http://npmjs.org/doc/scripts.html">scripts</a> setting in the package.json  where you can configure many items, among them being **start** and **prestart**. You simply add a command line action you'd like performed before starting the app in the **prestart** tag  and then **node app.js** in the **start** tag and you're ready to rock and roll. All you have to do is <code>npm start</code> and you're off to the races.

I have some coffeescripts I need transpiled before each run so my package.json looks a little like this:

{% highlight json %}
{
  "name": "app-name",
  "author": {
    "name": "George Shank"
  },
  "version": "0.1.1-3",
  "dependencies": {
    "express": "2.5.9",
    "jade": ">= 0.0.1"
  },
  "scripts": {
    "start": "node app.js",
    "prestart": "coffee -c public/js/*.coffee"
  }
}
{% endhighlight %}

There's a ton of potential here. You could do this with all your <a href="http://lesscss.org">Less files</a> as well or anything else you need done just before run time.

Happy hacking!

<h6>Thanks to <a href="http://twitter.com/supershabam">@supershabam</a> for showing this to me.</h6>
