---
layout: post
title: "No hassle Vim with Ruby support in Lion"
category: Development
tags: [vim, lion]
---
{% include JB/setup %}

Recently I've been trying to force myself to learn *Vim*. By using *Vim*. However there's a few things from *TextMate* that I really cannot live without. Like `command-t` for quickly opening a file using fuzzy matching.

So I found the conveniently named Vim plugin [command-t](https://wincent.com/products/command-t) from [Wincent Colaiuta](http://wincent.com). The plugin requires that Vim is compiled with Ruby support, which the default OS X installation of Vim is not.

Did some googling and found a few guides on how to recompile Vim with Ruby support. None of them worked for me, they all ended with some obscure GCC errors or something. And in the process I also managed to bork the default Vim installation.

Also found a few people suggesting that one should just use MacVim, that comes with Ruby support. The whole point of learning Vim is for me to be able to run Vim in the Terminal, in my TMUX environment.

But I just wanted to try the `command-t` plugin to see if it was worth the hassle of getting Vim with Ruby running. So I installed MacVim via brew just to try it out. The plugin worked very well.

Just for the fun I started digging around in the MacVim.app bundle. And there it was, a Vim executable! All I had to do was to setup an alias in my _.zshrc_!

{% highlight bash %}
    alias vim='/usr/local/Cellar/macvim/7.3-64/MacVim.app/Contents/MacOS/Vim'
{% endhighlight %}

No recompiling, no messing with default vim, just good ol' _brew_ and a little alias.

_Note that setting up a symlink will not work because the executable relies on being started from that path._