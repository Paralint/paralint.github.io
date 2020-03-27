---
author: ixe013
comments: false
date: 2011-03-16 03:06:17+00:00
layout: post
link: http://www.paralint.com/blog/2011/03/15/can-your-gina-do-this/
slug: can-your-gina-do-this
title: Can your GINA do this ? (running any process on the secure desktop)
wordpress_id: 152
categories:
- Other technical
- Security
- Windows
---

I get asked a lot of questions about my custom Gina. Most of them come from people who want to write a custom Gina themselves to do … whatever.

 

A custom Gina runs in Winlogon’s process. It runs under the SYSTEM account, in the TCB… In short it can do pretty much anything. But some things just can’t be done, no matter what rights you have.

 

Fortunately, there is an easy way to tell if a GINA can do what you need it to do, without having to write a single line of code. 

<!-- more -->

The trick is to launch a cmd.exe shell (or whatever shell you prefer) on the Winlogon desktop. I used to work with [EnumWinStaGui](http://www.ikriv.com/en/prog/tools/EnumWinstaGui/), but [Microsoft’s own psexec (of Sysinternals fame)](http://technet.microsoft.com/en-us/sysinternals/bb897553) is easier to use. 

 

To run a cmd shell on Winlogon desktop, do the following :

 

  
  1. Logon with an account with administrator rights 
   
  2. Open a command prompt (elevated if you are on Vista or later) 
   
  3. Type this command :        
     
    
    psexec –dsx cmd.exe


  





That’s it ! You will not see your shell, because it is running on the current desktop. To verify that it works, hit CTRL-ALT-DEL and your command prompt will be there. On Vista and later, it will be hidden under the full screen logon application (logonui.exe). Just ALT-TAB to it. That shell will keep running even if you log out, because it is not tied to the interactive session you used to start it. From that command line on the secure desktop, launch whatever command you need to confirm that what you want to do can be done. 





For example, say you need to launch a process that will notify a web application just before the password is validated, maybe to start a timekeeping application. Can it be done ? Yes. On the command line running on the secure desktop, type this command :






    
    start iexplore http://www.paralint.com/







It might not work. Maybe your proxy is not set up in the SYSTEM account ? Whatever the problem may be, you need to fix it. Writing a custom Gina is a waste of time if you can’t get past this step. 
