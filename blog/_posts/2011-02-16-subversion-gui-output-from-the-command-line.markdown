---
author: ixe013
comments: false
date: 2011-02-16 04:22:09+00:00
layout: post
link: http://www.paralint.com/blog/2011/02/16/subversion-gui-output-from-the-command-line/
slug: subversion-gui-output-from-the-command-line
title: Subversion GUI output from the command line
wordpress_id: 151
categories:
- Other technical
- Windows
---

I use Subversion command line client. But I also have [Tortoise SVN](http://tortoisesvn.tigris.org/) installed, because some operations like log and check-in benefit from the GUI.

 

Tortoise SVN is a Explorer shell extension which calls a Windows executable, TortoiseProc.exe. 

 

To use it from the command line, simply save this batch file somewhere in your path :

   
    
    @echo off 
    start "" "C:\Program Files\TortoiseSVN\bin\TortoiseProc.exe"  /command:%1 /path:"%2"







Then simply call like this :






    
    tortoise log http://src.pararlint.com/aucun/trunk







or like this






    
    tortoise commit .







Tested on Windows with 32 and 64 bits version.
