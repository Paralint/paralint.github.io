Any user can unlock now with this custom GINA
=============================================

Aucun is a replacement GINA that wraps Microsoft's own MSGINA.DLL to allow any given group of users to unlock or force logoff a locked session on a Windows machine, unless the currently loggon on user is a member of a group you specify. Feel free to give it a try! 

-   [Features](https://www.paralint.com/projects/aucun/index.html#Features)
-   [Download](https://www.paralint.com/projects/aucun/index.html#Download)
-   [Installation](https://www.paralint.com/projects/aucun/index.html#Installation)
-   [Unlock or force logoff ?](https://www.paralint.com/projects/aucun/index.html#Unlock+or+force+logoff+%3F)
-   [Exclusion (blacklist)](https://www.paralint.com/projects/aucun/index.html#Exclusion+%28blacklist%29)
-   [Auditing](https://www.paralint.com/projects/aucun/index.html#Auditing)
-   [Gina chaining](https://www.paralint.com/projects/aucun/index.html#Gina+chaining)
-   [Debugging](https://www.paralint.com/projects/aucun/index.html#Debugging)

Aucun exposes a complete terminal services aware GINA implementation. It delegates most of the security functionnality to the original Microsoft GINA, msgina.dll. It intercepts unlock requests and if the user trying to unlock is a member of a group you specified, the session is either terminated (force logoff) or unlock.

[

![](https://www.paralint.com/projects/aucun/architecture.png)

](https://www.paralint.com/projects/aucun/index.html#download)

Features
--------

I created this for a friend that needed an unlock feature. By popular demand, I added force logoff and warning display. Here is a more detailed feature list:

-   GUI provided by original MSGINA.DLL (no training of end user required)
-   Allows any member of a given group to force logoff a locked session
-   Allows any member of a given group to unlock a locked session
-   Support a exclusion group (to prevent unlocking administrators by regular users)
-   Allows to display a custom message when the workstation is locked
-   Supports 64 bits versions of Windows
-   Supports international versions of Windows
-   Allows chaining multiple Gina's together

If you need a feature that Aucun doesn't provide, [send me an email.](mailto:guillaume@paralint.com)

Windows XP SP2 and Windows Server 2003 supported only (32 or 64 bits)

Microsoft decided to remove support for custom GINA in Windows Vista. This GINA works on Windows XP SP2 and Windows Server 2003, but it will never work on Vista or Server 2008, by desing. I am working on a [Credential provider port](https://www.paralint.com/blog/2009/02/24/porting-a-custom-gina-to-a-credential-provider).

Download
--------

There are three ways to get the DLL. Help yourself to :

-   A [compiled binary](https://www.paralint.com/projects/aucun/dl/aucun-1.4.8.zip) (Aucun.dll and aucun64.dll)
-   The [source code snapshot](https://www.paralint.com/projects/aucun/dl/aucun-src-1.4.8.zip) in a zipped file
-   Source code from the Subversion repository

    svn checkout http://src.paralint.com/aucun/trunk/ aucun

-   Browse the [source code online](http://src.paralint.com/listing.php?repname=Aucun&path=%2Ftrunk%2F#path_trunk_)

Installation
------------

To install this replacement GINA, you must:

1.  Unzip and copy Aucun.dll in a directory that is writeable only by Administrators
2.  Create a registry key so that Windows will use the replacement GINA instead of msgina.dll
3.  Assign values to some registry keys, depending on what feature you want to enable
4.  Reboot

Windows 64 bits support

If you are using the 64 bits version of Windows XP or Windows Server 2003, follow the same intructions, but replace aucun.dll by aucun64.dll.

Security warning

If a regular user can replace this GINA with another DLL, they will be able to elevate their privilege and/or grab the password of anybody who logs on the machine. In short, put Aucun.dll in System32 and you'll be fine.

### Replacing Microsoft's Gina

Default Windows behavior is to load msgina.dll, unless a registry says otherwise. To make Windows load Aucun.dll, create a string value key named GinaDLL under HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon.

REGEDIT4

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon]
"GinaDLL"="aucun.dll"

Aucun.dll will find msgina.dll if you leave it where it is, even if you renamed you Windows directory (like WinNT, in case of an upgraded machine).

### Telling Aucun about your groups

There are 5 registry key you can set. Under HKEY_LOCAL_MACHINE\SOFTWARE\Paralint.com\Aucun\Groups you can set any of the three entries below to allow unlocking, force logoff or exclusion from both. Aucun is localized, so if you use a localized (non-english) version of Windows, use the localized name. For example, if you want to exclude the build-in Administrator group from my gina, you will use "Administrateurs" as a group name.

| Key | Purpose |
| --- | --- |
| Unlock | Members of this group will be able to unlock a workstation locked by anyone, unless the user who locked the session is a member of the "excluded" group. |
| Force logoff | Members of this group will be able to force logoff any other user, even if they are not administrators, unless the user who locked the session is a member of the "excluded" group. |
| Excluded | Members of this group will get standard MSGINA behavior. The replacement GINA *will not hook* the original GINA dialogs at all for members of this group. |

### Display a notice

The last two registry keys are used to display a warning message. The keys are under Under HKEY_LOCAL_MACHINE\SOFTWARE\Paralint.com\Aucun\Notice The message will only be shown when the user locks his session and the following conditions are met

-   The exclusion group is not set or the current user is not a member of it.
-   The unlock group is set (the group can be empty, I don't check for that).
-   The user lock his workstation using Ctrl-Alt-Delete. Windows-L is does not get the warning.

| Key | Purpose |
| --- | --- |
| Caption | The message box title, could be something like "Danger, Will Robinson !" |
| Text | The warning to be displayed, with an optional insert for the name of the group that can unlock. |

You can put one optional insertion point in the message (the text parameter in the table above). The first %s you put will be replaced by the name of the unlock group. You can force a newline with \n (use as many as you need). The user will be presented with a "Yes", "No", "Cancel" choice. Answering "yes" will logoff the user instead of locking.

For example, you can use "Danger, Will Robinson ! Members of the %s group can impersonate you ! Logoff instead ?". There is another example in the sample configuration.

### Sample configuration

In the source and binary distribution, there is a file named Sample.reg that you can modify to put your own groups and remove the features you don't like.

REGEDIT4

[HKEY_LOCAL_MACHINE\SOFTWARE\Paralint.com\Aucun\Groups]
"Unlock"="My domain\\The unlockers"
"Excluded"="Administrators"
"Force logoff"="My domain\\The unloggers"

[HKEY_LOCAL_MACHINE\SOFTWARE\Paralint.com\Aucun\Notice]
"Caption"="Security warning"
"Text"="Someone to unlock your workstation and impersonate you. Would you like to log off instead ?"

### Uninstall

Aucun.DLL will be held in memory by Winlogon. To uninstall, just remove the registry keys and reboot. You'll be able to delete Aucun.dll after that.

Unlock or force logoff ?
------------------------

When a user has locked his Windows session, there is no way to tell if any documents are open, let alone modified and not saved. An administrator can enter his username and password to unlock the session, at the risk of loosing unsaved documents. Regular users do not have that right. Event with full administrative rights on the machine, there is no way to unlock the session and return back to the original desktop, allowing to save the documents and logoff normally.

Microsoft's decision to allow only administrator to force logoff a session makes perfect security sense. Then the stupid reality hits.

On a shared computer, it might make more security sense to allow members of any arbitrary group to force logoff any other user than to put everybody in the Administrators group. Aucun answers that need. You set a group name in the registry (any existing group will do) and if you try to unlock a session using a regular account that is a member of this group, the currently logged on user will be logged off forcefully.

Unlocking a session is different. You setup another group, and any members of that group will be able to unlock the session as if it was theirs. No questions asked. That user can then save and close documents, and logoff. But they can also impersonnate the logged on user. Send email, read EFS encrypted files and whatnot.

Security warning

Allowing any user to unlock a workstation is a security hole in most scenarios. Before you enable this feature, make sure that the security risk of not being able to log on that specific computer outweights the security risk of a handfull of users impersonnating others.

Exclusion (blacklist)
---------------------

Whether you choose to enable force logoff, unlock or both, you can always configure an exclusion group. If the user locking his workstation is a member of that group, standard Windows rule apply: Only administrators will be allowed to force loggoff the session.

Auditing
--------

There is no auditing feature in Aucun. It uses the Windows API call LogonUser internally when you try to unlock the workstation, with the LOGON32_LOGON_UNLOCK flag. Any unlock attemps, failed or succeeded, will be logged according the system security policy. A typical event will look like this:

Event Type:	Success Audit
Event Source:	Security
Event Category:	Logon/Logoff
Event ID:	528
Date:		9/18/2008
Time:		12:09:55 PM
User:		PARALINT-SERVER\funny
Computer:	PARALINT-SERVER
Description:
Successful Logon:
 	User Name:	funny
 	Domain:		PARALINT-SERVER
 	Logon ID:		(0x0,0x2E0A1)
 	Logon Type:	7
 	**Logon Process:	Paralint**
 	Authentication Package:	Negotiate
 	Workstation Name:	PARALINT-SERVER
 	Logon GUID:	-
 	Caller User Name:	PARALINT-SERVER$
 	Caller Domain:	WORKGROUP
 	Caller Logon ID:	(0x0,0x3E7)
 	Caller Process ID: 324
 	Transited Services: -
 	Source Network Address:	-
 	Source Port:	-

Logon type "7" is LOGON32_LOGON_UNLOCK, and caller logon id 0x3E7 is the SYSTEM logon session (999). You see that the logon proces name is Paralint.

A GINA has complete control over your machine. It runs in the TCB and it sees passwords all day. There is no backdoor in this code, but I fully understand that you don't take my word for it. Spending the time to analyse the code for a backdoor is not an insult, it is actually an honour. If you want to work from the source and are having trouble setting it up, [send me an email.](mailto:guillaume@paralint.com) I will try to help you.

Gina chaining
-------------

Aucun relies on MSGINA.dll for many of its functions. Many Gina work the same. Aucun exports a number of functions for Winlogon to use, and simply forward calls to the original Gina. This is the general case that answers most use cases.

After a few requests, I decided to support chaining my Gina to any other Gina. To enable this feature, simply add this registry key:

REGEDIT4

[HKEY_LOCAL_MACHINE\SOFTWARE\Paralint.com\Aucun]
"Original Gina"="some_3rd_party.dll"

If the key is absent, I chain with Microsoft's Gina automatically. If you already have an entry (other than aucun.dll) under the GinaDLL key, then copy it over as-is to the "Original Gina" registry key.

I beleive the feature is pretty stable, thanks to helfull users who gave it a try and help me troubleshoot the problems they were facing. I cannot guarantee it will work with every single Gina out there, though.

Wich brings me to the next topic...

Debugging
---------

If for some reason the Gina does not work for you, drop me a line. I will help you and you will help me improve the documentation for everybody.

It will help to send me an output trace. To generate it, here is what you do :

1.  Download, unzip and run Microsoft's [DebugView](https://technet.microsoft.com/en-us/sysinternals/bb896647.aspx) (of SysInternal fame)
2.  From the Options menu, uncheck "Force carriage return"
3.  Add the following line to the registry

    REGEDIT4

    [HKEY_LOCAL_MACHINE\SOFTWARE\Paralint.com\Aucun\Debug]
    "Output"="OutputDebugString"
    "Level"="Info"

4.  Bundle the trace and description of your problem and [send me an email.](mailto:guillaume@paralint.com)
