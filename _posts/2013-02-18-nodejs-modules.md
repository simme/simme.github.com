---
layout: post
title: "Some Node.js Modules I Wrote"
category: NodeJS
tags: [node, js, modules]
---
{% include JB/setup %}

Such a long time since I wrote something here. And I've felt for a long time that I want to get some writing done. And since I really don't know what to write about; here is a quick run down of some recent _Node.js_ modules I wrote!

# Staticfy

[Staticfy](https://github.com/simme/node-staticfy) takes a string or buffer and writes it to a file that can be reached with the given URL. What I mean by that is that if this file was served from a regular Apache-installation, or similiar, it would have the specified URL.

Let  _html_ be a string of, well, HTML.
	
{% highlight javascript %}
staticfy(html, '/', '/var/www', callback);
{% endhighlight %}

Now this would create the file `/var/www/index.html` containing the _html_ we gave to _Staticfy_. Hence if the Apache docroot was at `/var/www` this file would be served from `http://ourdomain.com/`.

On the the list of features I want to add is support for streams.

# YAMLHead

[YAMLHead](https://github.com/simme/node-yamlhead) makes it easy-peasy to parse "_Jekyll-like_" markdown files with node.

It can handle both a file path and piped data! I'm too lazy to write an example for this one, but there's some stuff over at the GitHub repo!

# HTTPDigest Client

So I was gonna do some stuff with the API from [Tågtider](http://tagtider.net). It's a nice API and all, but they use HTTP Digest for authentication.

I did some _NPM browsing_ but could only find servers with digest support, no clients! So naturally I did as any real nerd would do, wrote it myself.

It is not sophisticated, it does not follow any specification and it's pretty hacky. But it solved my problems!

I struggled to implemented streams support, but ultimately gave up. There's some hassle involving preflight requests and handshakes and what not. Someday, perhaps, maybe, probably not. Pull requests welcome!

# Filemap

[Filemap](https://github.com/simme/node-filemap) is a real simple one. It takes an array of paths and returns an object where the keys are the path and the values are the contents of each respective path.

{% highlight javascript %}
var filemap = require('filemap');
filemap(['foo/fuu.txt', 'omg.lol'], function (files) {
  console.log(files);
});
{% endhighlight %}

Would return something akin to:

{% highlight javascript %}
{ 'foo/fuu.txt': < 08 23 23 23 42 >,
  'omg.lol': < 34 24 25 64 45 >
}
{% endhighlight %}

# Dirstream

Finally on this list is [Dirstream](https://github.com/simme/node-dirstream). It's a simple stream wrapper around `fs.readdir()`. So it takes a path to a directory and then starts emitting `data-events` for each file/directory in the target.

It also takes an optional filter-argument which is just a regex used to filter out files you want to ignore.

Is not compatible with the new `streams2` initiative. But will however respond correctly to `.pause()` and `.resume()`.

# That's it

For all my published (even those not mentioned here!!) _node_ modules head on over to [my NPM profile](https://npmjs.org/~simme). Thanks for reading!

Send a [Tweet](http://twitter.com/simmelj) if you have any questions or just want to tell me how bad my code sucks (I have a filter in Tweetbot for that).