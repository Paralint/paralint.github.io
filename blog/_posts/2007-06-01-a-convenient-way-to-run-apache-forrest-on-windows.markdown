---
author: ixe013
comments: false
date: 2007-06-01 21:14:03+00:00
layout: post
link: http://www.paralint.com/blog/2007/06/01/a-convenient-way-to-run-apache-forrest-on-windows/
slug: a-convenient-way-to-run-apache-forrest-on-windows
title: A convenient way to run Apache Forrest on Windows
wordpress_id: 13
categories:
- Other technical
---

I use Apache Forrest to generate what will someday be the homepage of paralint.com. I use "forrest run" most of the time, and "forrest site", "forrest clean" every now and then.

I wrote a little batch file that will launch forrest in a new window when the "run" option is used, and in the same window when any other option is used. Here it is :

    
    @echo off
    if "%1"=="run" goto NEWWINDOW
    
    call "%FORREST_HOME%\\bin\\forrest.bat" %*
    
    goto END
    
    :NEWWINDOW
    start "Forrest" cmd /C "%FORREST_HOME%\\bin\\forrest.bat" %*
    
    :END


You can also add a /T:2F switch to make the window white on green. I often have quite a few console windows running, separating them by color reminds me which one is which !
