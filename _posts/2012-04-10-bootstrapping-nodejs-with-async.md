---
layout: post
title: "Bootstrapping Node.js with Async"
category: NodeJS
tags: [node, development]
---
{% include JB/setup %}

When writing node applications it's easy to get lost in the callback jungle. Especially during the bootstrap phase of your app. You have lots of clients connecting to lots of servers and some of them depend on each other. This is ends up in code that's hard to read and a nightmare to rewrite.

Enter [Async](https://github.com/caolan/async)! **Async** is an awesome Node module to help you manage asynchronous sequences and functions.

Let's go with the bootstrapping process of a server application for our example. We probably need to load a configuration file, perhaps do some processing on that. We need to connect to various databases, perhaps **Redis** and **Mongo**. We might want to preload some assets into memory. We probably want to start some kind of server like **Express**. And most important of all, we probably want to do this in a sequence. 

For example, if we start **Express** before we've connected to all of our databases a user could in theory connect to our server before it's ready. This is of course a hypothetical scenario, but you get it.

Looking at the documentation for **Async** we can see that there are a few of the functions thar are or particular interest to us. Those are

	* series
	* auto
	* waterfall

**Series** takes a collection of functions and calls them in sequence. **Auto** is a bit more complex. It allows you to name the different functions and set up dependencies between them, more on that below. **Waterfall** is basically like **series** but passes the result of the previous functions to the next.

I will be focusing mostly on **series** because that's what I've found to work best for me. I will quickly mention the others though.

Going forward I assume you know how to use **npm** to get **Async** into your project.

## Going async

The way I usually structure my app is with some kind of singleton object that acts as the common state for the app. This object holds a reference to the **Express** app, database clients etc. I'll exclude that boilerplate code here though and leave it for another article.

Let's end the rambling and write some code shall we?

{% highlight javascript %}
    exports.App = (function () {
      async.series(
        // Use async.apply() to call our functions with additional arguments.
        // Without apply we would have to write an anonymous function and
        // within that one call our real function with arguments.
        { loadConfig: async.apply(_loadConfig, this)
        , connectRedis: async.apply(_connectRedis, this)
        , startExpress: async.apply(_startExpress, this)
      },
      // Called when async finishes all of our functions
      // or if an error occurs somewhere during the process.
      function (err, results) {
        if (err) {
          throw err;
        }
        
        console.log('Bootstrapped!');
      });
      
      /**
       * Load config.
       *
       * @param self
       *   Self is our app object, "this" from above.
       * @param callback
       *   We need to call "callback" to tell async that
       *   we are done and we should move on to the next
       *   function.
       */
      function _loadConfig(self, callback) {
        asyncFunctionToLoadConfig(function (err, config) {
          if (err) {
            // Tell async about an error
            callback(err);
          }
          
          // Set config to our "singleton" state
          self.config = config;
          
          // Tell async that it's ok to move on
          callback();
        });
      }
      
      function _connectRedis(self, callback) { ... }
      function _startExpress(self, callback) { ... }
    })();
{% endhighlight %}

As you can see it's pretty simple. Give async a number of functions and you're pretty much done! Just a few gotchas though.

The **series** completion callback will be called once all functions are run _or_ if an error occurs during one of the functions. If an error occurs execution is stopped and the **series** completion callback is called right away with the error. At least as long as you pass the error to the callback you're given.

## The other functions

So, what about **waterfall** and **auto**. They might fit your style better.

With **waterfall** you could have some kind of state object that you pass between the functions instead of passing a reference to self as I do. That might actually be a prettier way of doing it.

With **auto** you could potentially speed up the bootstrap process a little by running functions that don't depend on anyone else in parallell. Although I've found that this adds unnecessary complixty. Your bootstrap process should in optimal cases only run once, since your servers always have 100% uptime, right, **right**? Anyway I personally feel like it's a place where performance should be traded for simplicity. It doesn't really matter if your bootstrap process takes 100ms or 10ms.

## Conclusion

As you've hopefully seen, **Async** can be a great aid when doing asynchronous programming. Not only when doing bootstrapping. Go take a look at the docs and familiarize yourself with the other functions.

If you have any questions or feedback, like telling me I'm doing it wrong, please do so on [Twitter](http://twitter.com/simmelj)!