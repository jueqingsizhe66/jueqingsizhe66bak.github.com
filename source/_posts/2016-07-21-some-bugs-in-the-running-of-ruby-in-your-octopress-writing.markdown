---
layout: post
title: "Some bugs in the running of ruby in your octopress writing"
date: 2016-07-21 16:23:51 +0800
comments: true
categories: life
---

Problem below:

```
/usr/local/bin/rake:23:in `load`: cannot load such file --/usr/share/rubygems-integration/all/gems/rake-10.3.2/bin/rake(LoadErro)
    from /usr/local/bin/rake:23:in `<main>`
```

solution below:
<!--more-->

```
source /usr/local/rvm/scripts/rvm
```

because your rvm had been closed while needed, so rake command get error.
