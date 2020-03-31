---
author: ixe013
comments: false
date: 2011-12-09 04:52:01+00:00
layout: post
link: http://www.paralint.com/blog/2011/12/09/find-new-modified-and-unversioned-subversion-files-on-windows/
slug: find-new-modified-and-unversioned-subversion-files-on-windows
title: Find new, modified and unversioned Subversion files on Windows
wordpress_id: 171
categories:
- Other technical
- Windows
---

Nobody likes to break the build. When I do it, it is often because I forgot to add a file to the repository. The build server will not get it and the build will break.

This Windows batch file will parse Subversion's `svn up` output and show you what files were modified, but also what files should be added.

It looks for C++, C, H, PHP, Python, Java and then some. You can easily add your own to the list.

To use simply call localfiles.bat from any versionned directory. Anything you add to the command line will be passed along to `svn up`. Try these variations :



	
  * `localfiles.bat -u`` to see potential update conflicts.`

	
  * `localfiles.bat c:\the\path\to\my\project\sources`` works, you can run the command from anywhere`

	
  * `localfiles.bat --ignore-externals`` or any other Subversion command you can think of`


In the sample output (below) you will see

	
  * New Source Files are source that were added (localy) but never comitted.

	
  * Modified Source Files are source that are under source control and were modified locally.

	
  * Unversioned Source Files are source that probably should be under source control.

	
  * Each file is listed, with (no source file) if it looks ok.



```
$ localfiles.bat C:\Users\Guillaume\src\Projects\aucun.selfserve

Gathering data...

======================================
 New sourcefiles
======================================
(none found)

======================================
 Modified sourcefiles
======================================
M       C:\Users\Guillaume\src\Projects\aucun.selfserve\GINA\SecurityHelper.cpp
M       C:\Users\Guillaume\src\Projects\aucun.selfserve\GINA\loggedout_dlg.cpp
M       C:\Users\Guillaume\src\Projects\aucun.selfserve\common\Trace.c
M       C:\Users\Guillaume\src\Projects\aucun.selfserve\GINA\GinaHook.c

======================================
 Unversioned files
======================================
?       C:\Users\Guillaume\src\Projects\aucun.selfserve\GINA\StaticPrompt.cpp
?       C:\Users\Guillaume\src\Projects\aucun.selfserve\shellie\shellie_p.c
?       C:\Users\Guillaume\src\Projects\aucun.selfserve\shellie\dlldata.c
?       C:\Users\Guillaume\src\Projects\aucun.selfserve\shellie\shellie_i.c
?       C:\Users\Guillaume\src\Projects\aucun.selfserve\shellie\shellie.h
```

[Here is the file](/blog/wp-content/uploads/2011/12/localfiles.txt). I gave it a txt extension, in case you are behing a paranoïac corporate proxy.


