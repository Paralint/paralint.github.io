---
author: ixe013
comments: false
date: 2008-01-05 13:58:50+00:00
layout: post
link: http://www.paralint.com/blog/2008/01/05/unlock-any-users-session-without-losing-data-or-killing-processes/
slug: unlock-any-users-session-without-losing-data-or-killing-processes
title: Unlock any user's session without losing data or killing processes
wordpress_id: 37
categories:
- Security
- Windows
---

UPDATED Feb. 2nd 2008 : [There is an enhanced version](/projects/aucun/) out, with better code, features and documentation.


A friend of mine wanted a special group of users to be able to unlock a workstation without losing any data. Putting those uses in the administrator groups was not a solution, because the default behaviour of Windows was to close or terminate every process and give the administrator a brand new session. Fast user switching added some tricks, but in the end there was no way to recover an open document with modifications in it from a locked Windows workstation.

The [project has its own page](/projects/aucun/) now. If you are a computer historian, read on for the original first release.


<!-- more -->He asked me if I could write a screen saver to do this, but the answer was a little deeper under the hood, and a lot more fun. I wrote a replacement Graphical Identification and Authentication DLL (GINA DLL) that hooks the standard Microsoft Windows Gina DLL of Windows XP and Windows Server 2003. It also supports Terminal services sessions. It allows you grant members of a group (any group) to unlock any user's session. No need to be an administrator. Any user can unlock now (Aucun).


Microsoft replaced GINA DLLs with credentials providers (ICredentialProvider), somewhat like the Solaris pluggable authentication modules PAM. This Gina DLL will never work in Vista, by design.


Turns out it is pretty easy. You can [get the source code and a compiled binary here](/projects/aucun/). I recommend that you don't trust me with your password and work with the source. Anonymous Subversion access with this command

```
svn co [http://src.paralint.com/aucun/trunk](http://src.paralint.com/aucun/trunk) aucun
```

<!-- more -->

I started with the Gina Hook sample from the SDK, and added support for terminal services. Then I stripped away all the unnecessary hooks and updated the dialog detection code. A few registry calls later and some basic group membership checking later was all that was needed.

To allow any user from any given group to unlock a Windows session, you have to:



	
  1. Hook every function of the regular Gina, msgina.dll

	
  2. Export every function yourself, forwarding them all to msgina.dll as is except for WlxNegotiate and WlxInitialize

	
  3. In WlxInitialize, hook the Windows provided function WlxDialogBoxParam (so msgina.dll will call your DLL when it creates a dialog box)

	
  4. In your version of WlxDialogBoxParam, let everything go through except requests to create the unlock dialog

	
  5. Hook the DLGPROC and wait for WM_COMMAND with wParam == IDOK

	
  6. Check the user's password (LogonUser with LOGON32_LOGON_UNLOCK)

	
    * If it doesn't match, let the call go through the regular GINA

	
    * If it matches, and the user is part of the group you want to be able to unlock (CheckTokenMembership), call EndDialog with IDOK

	
    * It it matches and the user is not in the group you want to be able to unlock, let the regular GINA handle it





That's about it. The third bullet of item 6 is actually what will happen most of time. The same user that locked its station comes back and enters a password that matches, but that regular user is not part of the unlock group. MSGINA.DLL handles it and unlock the session as usual. (UPDATED january 8th 2008 ->) If you audit logon events, succesfull or failed, they will be logged under the name of the person unlocking the session, not the original user. It is a deterrent measure only, it will not give you non-repudiation of actions made after the session was unlocked.

I insists there is no need for the users to be members of the administrators group for this to work. With my implementation, you merely set a registry key with the name of the group you want to use. Add any user(s) or group(s) to this group.

```
HKEY_LOCAL_MACHINE\SOFTWARE\Paralint.com\Aucun"GroupName"="paralint.com\DG_SESSION_UNLOCKERS"
```

I might add a blacklist feature, so that members of the the administrative group can never be unlocked. It is not hard to do. If I see interest in this project, I write it happen.

The code is all plain old, good old C. Function pointers, hooked procedures. All the fun stuff .Net hides from you ;) Remember: Gina were introduced in Windows NT 3.51, some 15 years ago. And yes, I was already a Windows programmer back then...
