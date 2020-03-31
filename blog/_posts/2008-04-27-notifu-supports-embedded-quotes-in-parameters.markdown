---
author: ixe013
comments: false
date: 2008-04-27 02:42:16+00:00
layout: post
link: http://www.paralint.com/blog/2008/04/26/notifu-supports-embedded-quotes-in-parameters/
slug: notifu-supports-embedded-quotes-in-parameters
title: Notifu supports embedded quotes in parameters
wordpress_id: 51
categories:
- Updates
---

I fixed a bug in notifu that made it ignore quotes that were escaped with a backslash. For example, this command line works now :


```
notifu /m "\"Theo Est\" [test@example.com](mailto:test@example.com)"
```






Thanks to [Sof](http://www.sof-paradise.info/) for the heads up ! 
