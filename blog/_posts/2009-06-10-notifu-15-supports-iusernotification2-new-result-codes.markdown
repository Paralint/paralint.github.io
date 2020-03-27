---
author: ixe013
comments: false
date: 2009-06-10 01:09:39+00:00
layout: post
link: http://www.paralint.com/blog/2009/06/09/notifu-15-supports-iusernotification2-new-result-codes/
slug: notifu-15-supports-iusernotification2-new-result-codes
title: Notifu 1.5 supports IUserNotification2 new result codes
wordpress_id: 118
categories:
- Updates
---

I updated my Notifu utility to use the new IUserNotification2 interface introduced in Vista. It allows to detect a left or right click on the icon in the system notification area. If you run Windows XP, behaviour is unchanged.

On Vista, you can also revert to the old interface by adding the /xp switch.

I am also investigating a timeout problem. In short, the timeout is not honoured if the user is doing something. A system default is used. On my Windows XP SP2 and Vista Business SP1, it is 10 seconds. Some users report shorter times than that (3 seconds).

You can [download it here](http://www.paralint.com/projects/notifu/download.html). 
