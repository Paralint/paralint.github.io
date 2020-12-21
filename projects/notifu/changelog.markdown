---
layout: page
title: Release history and change log
---

# Changelog
Here is the version information and archived versions of Notifu. You can download past releases from here, too, by clicking on the following icons:

 - ![](compress.png) To download a ready to use binary (in a zip format)
 - ![](page_white_visualstudio.png) To download zipped source code


## Subversion access
If you want to modify the source, please use my Subversion server directly. You can get the trunk revision with this command:

```
svn co http://src.paralint.com/notifu/trunk notifu
```

| Version | Binary | Source | Date | Trunk revision | What's new |
| 1.7.1 | [![Download compiled binary (ready to use EXE)](compress.png)](dl/notifu-src-1.7.1.zip)|[![Download zipped source](page_white_visualstudio.png)](dl/ntofiu-1.7.1.zip) | February 15, 2010 | 74 | Icon next to message can be removed by adding switch /t none |
| 1.7 | [![Download compiled binary (ready to use EXE)](compress.png)](dl/notifu-src-1.7.zip)|[![Download zipped source](page_white_visualstudio.png)](dl/ntofiu-1.7.zip) | 	October 20, 2019 | 73 | Now displays its BSD 3-clause licence more clearly, with a mention in every source file (to help with node-notifier licensing.  |
| 1.6.1 | [![Download compiled binary (ready to use EXE)](compress.png)](dl/notifu-src-1.6.1.zip)|[![Download zipped source](page_white_visualstudio.png)](dl/ntofiu-1.6.1.zip) | March 6, 2017 | 72 | Adjusted the "FileDescription" field that was displayed on Windows 10 anniversary edition |
| 1.6 | [![Download compiled binary (ready to use EXE)](compress.png)](dl/notifu-src-1.6.zip)|[![Download zipped source](page_white_visualstudio.png)](dl/ntofiu-1.6.zip) | February 15, 2010 | 72 | New command line switches -q (to display without a sound) and -w (tries to bypass the 1st logon quiet time) |
| 1.5 | [![Download compiled binary (ready to use EXE)](compress.png)](dl/notifu-src-1.5.zip)|[![Download zipped source](page_white_visualstudio.png)](dl/ntofiu-1.5.zip) | June 8, 2009 | 63 | Uses the new IUserNotification2 interface on Vista and later. You can revert to Windows XP behavior by adding /xp to the command line. Also supports 3 new error codes : right and left click on the tray icon, and a balloon being dismissed by a new instance of Notifu. Optionnal debug output. See debug.reg in the soure package for details. |
| 1.4.1 | [![Download compiled binary (ready to use EXE)](compress.png)](dl/notifu-src-1.4.1.zip)|[![Download zipped source](page_white_visualstudio.png)](dl/ntofiu-1.4.1.zip) | October 10, 2008 | 44 | Supports embeeded \n that will be translated into carriage returns. |
| 1.3 | [![Download compiled binary (ready to use EXE)](compress.png)](dl/notifu-src-1.3.zip)|[![Download zipped source](page_white_visualstudio.png)](dl/ntofiu-1.3.zip) | March 2, 2008 | 27 | Supports concatenation of multiple /m and /p options. Usefull when you try to insert parameters filled in by other programs. |
| 1.2 | [![Download compiled binary (ready to use EXE)](compress.png)](dl/notifu-src-1.2.zip)|[![Download zipped source](page_white_visualstudio.png)](dl/ntofiu-1.2.zip) | February 26, 2008 | 26 | Added support for embeded quotes in parameters. For example /m "Hello, \"World\"." now works. |
| 1.1 | [![Download compiled binary (ready to use EXE)](compress.png)](dl/notifu-src-1.1.zip)|[![Download zipped source](page_white_visualstudio.png)](dl/ntofiu-1.1.zip) | February 4, 2008 | 22 | Tested to work under Vista. Added support (and auto detection) of a timeout value in seconds. Now /d 3000 is the same as /d 3. Better error handling if IUserNotification is not supported. |
| 1.0 | [![Download compiled binary (ready to use EXE)](compress.png)](dl/notifu-src-1.0.zip)|[![Download zipped source](page_white_visualstudio.png)](dl/ntofiu-1.0.zip) | June 26, 2007 | 15 | Initial release (first seen on Google code). |
