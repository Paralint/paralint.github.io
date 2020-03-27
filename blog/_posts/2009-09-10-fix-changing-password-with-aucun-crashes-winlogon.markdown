---
author: ixe013
comments: false
date: 2009-09-10 02:11:20+00:00
layout: post
link: http://www.paralint.com/blog/2009/09/09/fix-changing-password-with-aucun-crashes-winlogon/
slug: fix-changing-password-with-aucun-crashes-winlogon
title: 'FIX : Changing password with Aucun crashes winlogon'
wordpress_id: 124
categories:
- Updates
---

There is an important update to aucun that fixes a bug in the dialog procedure hooking code. If you have Aucun version 1.4.2 or earlier, you will experiment the following bug :



	
  1. Login to the pc.

	
  2. Hit CTRL-alt-Del to get to the Windows Security Screen

	
  3. Click "Change Password"

	
  4. Click "Cancel" to get back to the main Windows Security Screen.

	
  5. Click "Lock workstation"  At this point, the workstation won't lock.

	
  6. Click "Cancel".


At this point Winlogon will crash and the pc will reboot. There are other ways to make the computer crash because of the same bug.

Please update to [version 1.4.3](http://www.paralint.com/projects/aucun/#Download).

Many thanks to Abdul Khaliq for helping me test and debug this release !
