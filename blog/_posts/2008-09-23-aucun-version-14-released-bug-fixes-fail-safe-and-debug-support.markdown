---
author: ixe013
comments: false
date: 2008-09-23 04:03:12+00:00
layout: post
link: http://www.paralint.com/blog/2008/09/23/aucun-version-14-released-bug-fixes-fail-safe-and-debug-support/
slug: aucun-version-14-released-bug-fixes-fail-safe-and-debug-support
title: 'Aucun version 1.4 released : bug fixes, fail safe and debug support'
wordpress_id: 65
categories:
- Updates
---

I just put online [version 1.4](http://www.paralint.com/projects/aucun/index.html#Download) of my replacement Gina ! Thanks to everyone who gave me a break while I was spending more time house shopping, buying and renovating. This release is very good, thanks to everybody who wrote me about problems they were facing... Here is what's new :

  * Fixed a bug where registry keys and groups had to be present to work.  
  * Fail safe behaviour reverts to normal MSGINA.dll if anything goes wrong  
  * Better detection of the user logged in coming back to unlock 
  * Registered as a logon process (Paralint shows in the Event log instead of Winlogon)  
  * Added an option to generate a debug output (off by default, see Sample.reg)  
  * Corrections and clarification in the documentation  
  * Automated build, test and release scripts 

Next up ? I am not sure. I am thinking about a self-service application, like a companion product, and when I get that to work, find a way to integrate that concept with Aucun. Something like after N bad logon, you are redirected to the self service application.

Enjoy !
