---
author: ixe013
comments: false
date: 2008-01-05 13:57:39+00:00
layout: post
link: http://www.paralint.com/blog/2008/01/05/denial-of-service-on-vista-using-resource-monitor/
slug: denial-of-service-on-vista-using-resource-monitor
title: Denial of service on Vista using Resource Monitor
wordpress_id: 36
categories:
- Windows
---

Microsoft wants you to run with lower privileges. They went out of their way in Windows Vista. You are a member of the Administrative group in Vista, but you the group is for deny only in your token. When you elevate, you get a new token without that deny group. Just like an administrator removing its newbie mask.

But I don't run Vista like that: I run it the hard way. I am a regular user, and in the rare cases I need it, I switch over to another account that has administrative privileges. I disabled user account control (UAC) alltogether.

If you're like me, try this 2 step trick to render you Vista slow or completely DOS (denial of service) depending on CPU and memory.


Save you work before you try this, you might have to reboot...


Bring up the task manager with CTRL-SHIFT-ESC. Click the "Resource Monitor". Watch your screen flicker as your CPU goes up and your battery goes down...
