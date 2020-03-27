---
author: ixe013
comments: false
date: 2008-05-05 19:51:21+00:00
layout: post
link: http://www.paralint.com/blog/2008/05/05/notifu-supports-concatenation-of-parameters/
slug: notifu-supports-concatenation-of-parameters
title: Notifu supports concatenation of parameters
wordpress_id: 52
categories:
- Updates
---

This [Notifu update](http://www.paralint.com/projects/notifu/download.html#Download) allows you to concatenate multiple /m and /p switches. It is usefull when a paramater to Notifu is feed by a program you have no control over.

For example, this command line now works :

    
    notifu /p Concatenate /p " this" /m "Hello" /m ", " /m "World"


Nothing is added to your parameters. If you want a space, you must add it.

Note to self : Got to fix my release script... A simple update takes longer to post online than to code !
