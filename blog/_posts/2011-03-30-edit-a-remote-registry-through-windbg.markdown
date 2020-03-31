---
author: ixe013
comments: false
date: 2011-03-30 03:02:02+00:00
layout: post
link: http://www.paralint.com/blog/2011/03/29/edit-a-remote-registry-through-windbg/
slug: edit-a-remote-registry-through-windbg
title: Edit a remote registry through Windbg
wordpress_id: 165
categories:
- Other technical
- Windows
---

I found a way to edit the registry while under a remote Windbg session. !dreg allows you to read the registry, but I had added a corrupt authentication package to the Lsa list in the registry that I had to remove. I found out the hard way that LSASS will load all authentication packages listed, even if you boot in safe mode.

Fortunately, I had [set up LSASS to run under ntsd](http://blogs.msdn.com/b/alejacma/archive/2007/11/13/how-to-debug-lsass-exe-process.aspx), which was connected to a remote Windbg.

To edit the registry of a remote machine running under a debugger :



	
  1. Break into the debugger. This step will happen naturally in most snafu ![Winking smile](/blog/images/{{ page.date | date: "%Y-%m-%d" }}-{{ page.slug }}/wlEmoticon-winkingsmile.png)

	
  2. Start a shell with the .! command

	
  3. Fix the registry with the command line reg.exe tool. For example, to restore authentication packages type

```
reg add HKLM\SYSTEM\CurrentControlSet\Control\Lsa /v "Authentication packages" /t REG_MULTI_SZ /d msv1_0
```



	
  4. Type exit to quit the shell (hit Enter enough times to get back to Windbg’s prompt)


Then use the g command to resume execution.

As a side note : The windows subsystem is fully loaded before LSASS.exe starts, or at least there is enough of it to launch CMD.exe and REG.exe.
