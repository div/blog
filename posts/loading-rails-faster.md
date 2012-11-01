---
title: Loading rails faster with ruby 1.9.3-p286 and performance patches
date: '2012-11-01'
description:
categories:
---

You'd definitely like to see rails starting faster, and even more important rspec to spin off all the tests without any startup penalties(only if you are not loading rails, of course).

One of the things you can do is to build ruby with performance patches by [funny-falcon](https://github.com/funny-falcon). I've been on the conference in Moscow where he gave the [talk](http://live.digitaloctober.ru/embed/1419?language=ru&params[pw]=635&params[ph]=355&params[episodes_under]=1&params[ext_css]=do_player&params[ext_js]=do_player&params[nolqtext]=1&params[state]=pause#time1347692940) about all the recent ruby patches he made. And this guy is really smart and knows his way thorough ruby internals really well. I could hardly understand half of his talk.
I've found the results of applying this patches to be pretty impressive:

```
time (rails runner "puts 'hello world'")

# 1.9.3-p194
# without patch:
4.80s user 0.92s system 87% cpu 6.555 total
# with patch:
2.73s user 0.83s system 82% cpu 4.299 total

# 1.9.3-p286
# without patch:
3.74s user 0.82s system 83% cpu 5.476 total
# with patch:
2.28s user 0.71s system 85% cpu 3.496 total
```

You can find how to build ruby with those patches in this [gist](https://gist.github.com/3826116).
There is one particular feature in the gist

```
CONFIGURE_OPTS="--disable-install-doc --with-readline-dir=$(brew --prefix readline)"
```

which is telling ruby-build to build ruby with readline support which would let you use utf-8 characters in irb. It seems like this is only an OSX [issue](http://blog.rlmflores.me/blog/2012/04/25/adding-utf-8-support-to-rubies-compiled-through-ruby-build/) .