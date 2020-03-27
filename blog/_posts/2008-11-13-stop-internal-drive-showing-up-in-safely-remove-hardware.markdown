---
author: ixe013
comments: false
date: 2008-11-13 04:27:19+00:00
layout: post
link: http://www.paralint.com/blog/2008/11/13/stop-internal-drive-showing-up-in-safely-remove-hardware/
slug: stop-internal-drive-showing-up-in-safely-remove-hardware
title: Stop internal drive from showing up in "Safely remove hardware"
wordpress_id: 82
categories:
- Other technical
- Windows
---

Like many of you, I had a drive that showed up in the "Safely remove hardware" tray icon, and was unable to remove it.

The trick is to subtract 4 from the Capabilities in the registry. Not easy, but it can be done. The only thing is that it keeps coming back after every boot ! And it looks like the value cannot be edited under Vista. Here is how to fix it for good.

<!-- more -->


### What has to be done (but doesn't work)


Find the drive that shows up in the safely remove hardware icon in your registry. It will be somewhere under HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Enum\IDE\. My DVD drive was here :

    
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Enum\IDE\CdRomHL-DT-ST_DVDRAM_GMA4082Nj_______________PM01____


There is a numerical an alphanumerical key under that with a DWORD Capabilities value.

![image](http://www.paralint.com/blog/wp-content/uploads/2008/11/image.png)

It is a bit field. The bit 0010 (4 in decimal) is the REMOVABLE bit. Clear it by subtracting 4 from the value you have. The value I had was 6, so now I am down to 2.

If you try to do that, you will get an access denied. That's ok, we will get around that. For now, right click on the key name and copy it (the key named 5&3392c8c4&0&0.0.0 in my example).


### Same idea, that works manually under Vista


You need to have the TCB privileged enabled to modify that registry key. Running regedit as an administrator (or elevated, if you have UAC) will not work. You can fix the value with this command line :

    
    psexec -s reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Enum\IDE\CdRomHL-DT-ST_DVDRAM_GMA4082Nj_______________PM01____\5&3392c8c4&0&0.0.0" /v Capabilities /t REG_DWORD /d 2 /f


You will have to replace the key name with what you found in your registry. Running psexec -r regedit might work, but who wants to do that every time the computer boots ?


### Permanent fix


I hardly ever run as an administrator, so putting the above command in a script was out of the question. The solution was to create a Task using Windows Task Scheduler. I set it to run at startup, under the SYSTEM account. Don't forget the /f (force) flag, or else the job will appear to hang, waiting a confirmation from you.

Create a new task. In the "General" tab, click Change User or Group and enter "SYSTEM" . Trigger on startup and enter the same command as above, but without psexec -s. Your task is running reg.exe, with everything right of that (in the command line, earlier) as optional arguments.

![image](http://www.paralint.com/blog/wp-content/uploads/2008/11/image1.png)

Good luck and HTH !
