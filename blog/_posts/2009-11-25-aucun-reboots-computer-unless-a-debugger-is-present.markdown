---
author: ixe013
comments: false
date: 2009-11-25 05:02:53+00:00
layout: post
link: http://www.paralint.com/blog/2009/11/25/aucun-reboots-computer-unless-a-debugger-is-present/
slug: aucun-reboots-computer-unless-a-debugger-is-present
title: Aucun reboots computer, unless a debugger is present
wordpress_id: 130
categories:
- Updates
---

There is a bug in my replacement GINA. I left a call to [DebugBreak](http://msdn.microsoft.com/en-us/library/ms679297%28VS.85%29.aspx) in my [initialisation code](http://src.paralint.com/diff.php?repname=Aucun&path=%2Ftrunk%2FGinaHook.c&rev=0).

 

When Winlogon can't load a GINA, the server reboots. I run all my tests in virtual machines attached to a remote kernel debugger. I had set up a rule to ignore this first hardcoded breakpoint. In a regular environment, Winlogon did the only sensible thing to do when it received that unhandled exception : terminate. 

 

That triggered the reboot process.

 

Please use version 1.4.5. I have updated the [project page](http://www.paralint.com/projects/aucun/), [binary](http://www.paralint.com/projects/aucun/dl/aucun-1.4.5.zip) and [source code snapshot](http://www.paralint.com/projects/aucun/dl/aucun-src-1.4.5.zip). I also [updated my build script](http://src.paralint.com/diff.php?repname=Aucun&path=%2Ftrunk%2Fmakezip.cmd&rev=171) so that it never happens again.
