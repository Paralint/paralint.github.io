---
author: ixe013
comments: false
date: 2011-03-26 05:11:12+00:00
layout: post
link: http://www.paralint.com/blog/2011/03/26/how-to-debug-a-credential-provider-locally/
slug: how-to-debug-a-credential-provider-locally
title: How to debug a Credential Provider locally
wordpress_id: 163
categories:
- Other technical
- Windows
---

Here is a quick and easy way to debug a Credential Provider running on your development machine, without needing to set up a kernel debugging session with two computers. Before you go down this road, let me tell you a little bit about LogonUI.exe behavior (as seen on Windows 7 ultimate SP1 64 bits) set to require CTRL-ALT-DEL to log on.

 

  
  * Every time you type CTRL-ATL-DEL, a new LogonUI process is launched. 
   
  * LogonUI will try to load any registered Credential Providers COM objects. 
   
  * You can [run any process on the secure desktop](/blog/2011/03/15/can-your-gina-do-this/)
 

With that knowledge, it is easy to set up a debugging session for your Credential Provider, right on your development machine. Before I continue, be aware that it might affect the stability of your computer temporarily, as the following illustration shows. 

 

![6015610](/blog/images/{{ page.date | date: "%Y-%m-%d" }}-{{ page.slug }}/6015610.jpg)

<!-- more -->

To debug your credential provider, you will need this :

 

  
  * A CredentialProvider that can be loaded by LogonUI 
   
  * Microsoft’s [Debugging Tools for Windows](http://msdn.microsoft.com/en-us/windows/hardware/gg463009.aspx)
   
  * Microsoft’s [psexec](http://technet.microsoft.com/en-us/sysinternals/bb897553) tool (of Sysinternals fame) 
 

I guess you could also use Visual Studio instead, ymmv. If it works, please drop us a line in the comments.

 

To start debugging, here is what you have to do:

 

  
  1. Start a [command shell on the Secure Desktop](/blog/2011/03/15/can-your-gina-do-this/) with this command:         
     
```
psexec -dsx cmd.exe
```

  


  
  2. Build a debug version of your Credential Provider and register it. 


  
  3. Type CTRL-ALT-DEL to switch to the secure desktop. That will also launch LogonUI.exe 


  
  4. Hit Alt-Tab to switch to the command prompt you started at step 1 


  
  5. Change to the directory where your source code is 


  
  6. Debug LogonUI.exe and set the source path at once, with this command : 
      


    
```
windbg –pn logonui.exe –srcpath %CD%
```

  





That’s it. You might not be very familiar with windbg, so here are a few tips to get you started:






  
  * When you attached to LogonUI at step 6, the process is stopped. You can enter commands to set breakpoints before resuming it. 


  
  * Verify that your Credential Provider is loaded with the lm command. Look for a string like this one: 

      


    
```
000007fe`f5e80000 000007fe`f5e9b000 SampleCredentialProvider (private pdb symbols)
```

  


  
  * Just to be sure, verify that a specific symbol is loaded with this command (you should get an address): 
      


    
```
x SampleCredentialProvider!CSampleProvider::SetUsageScenario
```

  


  
  * Set a breakpoint to the same location with this command: 
      


    
```
bu SampleCredentialProvider!CSampleProvider::SetUsageScenario
```

  


  
  * Resume LogonUI with the g command. 





You can now lock your workstation and try to unlock it. When LogonUI appears to freeze, ATL-TAB to the debugger. It should be waiting for you with the source file loaded, waiting for your instructions. Type g to resume. Complete the unlock procedure to end LogonUI. 





To reattach to logon UI, you can quit windbg and launch it again, but it is easier to list the process with the .tlist command (LogonUI.exe will be the last in the list). Attach to it again with .attach 0n2331 (replace with your PID).





Happy debugging !









Image credits : [http://www.123rf.com/photo_6015610_idiot-saws-branch.html](http://www.123rf.com/photo_6015610_idiot-saws-branch.html)
