---
author: ixe013
comments: false
date: 2007-06-19 17:28:46+00:00
layout: post
link: http://www.paralint.com/blog/2007/06/19/loadlibrary-fails-when-impersonating/
slug: loadlibrary-fails-when-impersonating
title: LoadLibrary fails when impersonating
wordpress_id: 15
categories:
- Windows
---

I was playing around with SSPI, the Security Support Provider Interface. I stumbled across a behavior that I cannot explain : you cannot call LoadLibrary when you are impersonating.

If you check you program with SysInternals (now Microsoft's) [Process Monitor](http://www.microsoft.com/technet/sysinternals/ProcessesAndThreads/processmonitor.mspx), you'll find an error saying"Bad Impersonation".

LoadLibrary returns NULL and a call to GetLastError() says "Either a required impersonation level was not provided, or the provided impersonation level is invalid".

If you just specify a filename, LoadLibrary will look in the current directory only. If supply a full path to the DLL, LoadLibrary will find it but the CreateFile will fail with Bad Impersonation.

I am still baffled by this behavior. I will keep looking into it. Meanwhile, here is a [simple program that reproduces the problem](http://src.paralint.com/spikes/badimpersonation/BadImpersonation/BadImpersonation.cpp).


[BadImpersonation.cpp](http://src.paralint.com/spikes/badimpersonation/BadImpersonation/BadImpersonation.cpp)


You can get the full project using anonymous subversion :

`svn co [http://src.paralint.com/spikes/badimpersonation](http://src.paralint.com/spikes/badimpersonation) BadImpersonation`

Good luck !
