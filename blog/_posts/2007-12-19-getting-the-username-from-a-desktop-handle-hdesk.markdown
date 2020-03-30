---
author: ixe013
comments: false
date: 2007-12-19 19:35:20+00:00
layout: post
link: http://www.paralint.com/blog/2007/12/19/getting-the-username-from-a-desktop-handle-hdesk/
slug: getting-the-username-from-a-desktop-handle-hdesk
title: Getting the username from a desktop handle (HDESK)
wordpress_id: 33
categories:
- Security
- Windows
---

I was struggling with an strange error message, trying to retrieve a username from a desktop handle (HDESK). The Windows function LookupAccountName would always return error code 0x534 (that's 1332 in decimal). Looking it up with GetLastError gave this :

 

No mapping between account names and security IDs was done

 

It would have happened with a Windows Station handle (HWINSTA) also. It turns out there is a sensible and documented reason for that. But since I had to found out the hard (as in fun) way, I am posting it here.

 

If you are familiar with the ways SID (security identifiers) are handed out when you logon, here is the reason straight up :

 

The SID associated with desktops and window stations is not the SID of the logged on user, but the logon SID, a SID generated at logon time that identifies exactly and only this logon session. No account name is mapped to this transient SID, hence the error message.

 

Still confused ? Read on.


<!-- more -->
  

 

## Security descriptor crash course

 

Consider this : you open a remote desktop connection (terminal server) to a server and log in. You have a logon session on that machine. Keeping that session alive, you walk up to the server and log-in again, interactively. You now have 2 logon sessions on that machine. How does windows prevents one session from interfering with the other?

 

Well, security is always about assigning security descriptor (SECURITY_DESCRIPTOR) to things. A security descriptor is a discretionary access control list (DACL) made up of access control entries. Each ACE is a combination of allowed or denied access rights for a SID. A System Access Control list (SACL) that controls auditing and a few flags are also present, but it doesn't matter to us now. So each desktop and window station (and any other object for that matter) are assigned something like this :

 

![SECURITY_DESCRIPTOR](/blog/images/{{ page.date | date: "%Y-%m-%d" }}-{{ page.slug }}/sd.png)

 

So we have a security descriptor, what will we use to authenticate against ? Our process or thread token, of course.

 

## You are your logon session (any of them)

 

A token is like a conceptual pointer to a logon session. That logon session contains a unique logon identifier (LUID), the users SID and another SID, generated on the fly, that uniquely identifies a particular logon session. Here is a schematic view of the logon sessions and their SID:

 

![Two logon session gives two logon SID](/blog/images/{{ page.date | date: "%Y-%m-%d" }}-{{ page.slug }}/logon_session.png)

 

You can see that the user SID is the same in the two logon sessions, but the Logon SID is different. Only the User SID is mapped to the principal (Alice in this example).

 

Keith Brown has a tool that lets [view and change the security descriptor of the current window station and desktop](http://www.pluralsight.com/tools.aspx). Running this tool (in the logon session with logon SID S-1-5-5-0-218443) shows this:

 

![Running winstadacl to show the user SID and logon SID](/blog/images/{{ page.date | date: "%Y-%m-%d" }}-{{ page.slug }}/winstadacl.png)

 

To prevent one window station from interfering with the other, Windows uses the granularity of the security API to its advantage (who wouldn't?). Processes and threads from each logon have a token pointing back to the logon session they were started from. The desktop and window station are assigned a security descriptor that doesn't grant or deny any rights to the users SID. Rights are granted to the logon SID.

 

## What about my 0x534 error code?

 

For a reason I don't really understand, the logon SID is not granted any name. When you crawl you way from a HDESK or (HWINSTA) handle, using GetUserObjectInformation and LookupAccountName, you will get error 0x534. It says "No mapping between account names and security IDs was done" because there never was a mapping of the logon SID with a name to begin with! ;)
