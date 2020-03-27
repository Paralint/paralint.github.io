---
author: ixe013
comments: false
date: 2011-03-24 13:08:08+00:00
layout: post
link: http://www.paralint.com/blog/2011/03/24/unlocking-another-users-session-using-credential-providers/
slug: unlocking-another-users-session-using-credential-providers
title: Unlocking another user's session using Credential Providers
wordpress_id: 153
categories:
- Authentication
- Other technical
- Windows
---

I have been working a little bit lately on a Credential Provider port of my custom GINA. I did some tests, I poked around the API and I whipped together something I could load and play with. The [route I first thought of taking is still the right one](/blog/2009/02/24/porting-a-custom-gina-to-a-credential-provider/), but I ran into some unexpected problems.

Microsoft new architecture is better, but the separation it enforces makes it hard to play tricks with the security policy. First, there are no shortcuts with Credential Providers. The [documentation](http://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=ShellRevealed&DownloadId=7341) is formal :


<blockquote>CredentialProviders are not enforcement mechanisms. They are used to gather and serialize credentials. The Local Authority and Authentication Packages enforce security.</blockquote>


So we have to use an authentication package with the credential provider. Nothing that we didn't see coming. I have played with authentication packages before, it just a matter of writing one.

Second, LogonUI will not kill another user's session. I am able to get a credential provider tile on the unlock screen, logon with administrator credentials, but then I get this error message :


<blockquote>This computer is locked. Only the logged on user can unlock the computer.</blockquote>


Which makes some sense: why kill (or unlock) a user's session if you can just start a new session next to it ? Unfortunatly, it does not help the most common use case my custom GINA users need to solve: many users need to access a single application running in a single session.

But I haven't given up yet.

I tested on Windows 7 Ultimate 64bits, with or without fast user switching. If you ever able to unlock another user's session, write a line in the comments. I would love to hear about it.


