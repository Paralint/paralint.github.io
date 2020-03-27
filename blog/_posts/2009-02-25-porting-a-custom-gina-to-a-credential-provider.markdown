---
author: ixe013
comments: false
date: 2009-02-25 02:27:36+00:00
excerpt: Making a replacement Gina behave like a Credential Provider (not the other
  way around) looks like the ticket to have a single source solution to a Gina and
  Credential Provider.
layout: post
link: http://www.paralint.com/blog/2009/02/24/porting-a-custom-gina-to-a-credential-provider/
slug: porting-a-custom-gina-to-a-credential-provider
title: Porting a custom Gina to a Credential provider
wordpress_id: 108
categories:
- Authentication
- Other technical
- Windows
---

I am still amazed by the popularity of [my replacement Gina](/projects/aucun/) (it allows any user to unlock). Most users are quite happy using it with Windows XP and Server 2003, but I do get the occasionnal request for a Vista port. I am looking forward to doing it, but there are important desing changes that I must work with. I think I found a way to have a single source solution Gina and Credential Provider, with a clean architecture on any Windows version.

<!-- more -->Credential Providers cannot make an authentication decision. They only collect credentials. The approach I took in "Aucun" is that the Gina actually makes the authentication decision. That will not work under the new Credential provider architecture.

When I started looking to port my Gina to a Credential Provider, I first though of saving the initial credentials (when the first user logs on), then check any other user's credential in my own Credential Provider, and if all is good, replay the initial credentials.

That bothered on many grounds :



	
  * There will be a unlock event logged to the initial user, although that user never unlocked the workstation

	
  * I don't like saving passwords, even to encrypted memory

	
  * That architecture didn't look like something that would last


Looking deeper into the Credential Provider interface, I saw that there is one decision a Credential Provider can make. It can select what Authentication Package will honor the logon request (or unlock request, in my case). That takes place in my (your) [ICredentialProviderCredential::GetSerialisation](http://msdn.microsoft.com/en-us/library/bb776026(VS.85).aspx), by setting the ulAuthenticationPackage field of the [CREDENTIAL_PROVIDER_CREDENTIAL_SERIALIZATION](http://msdn.microsoft.com/en-us/library/bb773242(VS.85).aspx) structure.

So I believe the right way of doing this is to have two components :



	
  1. An authentication package that implements the unlock policy (any user can unlock, depending on group membership).

	
  2. A credential provider that collects credentials (and while you're at it, the session you want to unlock) and instruct winlogon to forward them the "unlock" authentication package.


That architecture fixes all three points above. And as a bonus, I can greatly simplify my existing Gina by merely collecting the credentials and forward them to the same authentication package. No need to hook dialog procedures and sending magic, reversed engineered constants to winlogon. In short, instead of making the Credential Provider look like a Gina, I make the Gina behave like a Credential Provider.

Authentication packages are the same under Vista and XP, that's a good thing. But to [debug them](http://blogs.msdn.com/alejacma/archive/2007/11/13/how-to-debug-lsass-exe-process.aspx), you need the scary kernel remote debugging tools. You also need to reboot every time you mess up. But I have a working authentication package from CVS NT code source, and another called [YPAuth](http://www.cse.unsw.edu.au/~matthewc/) (YP means yellow pages, the [old name of NIS](http://en.wikipedia.org/wiki/Network_Information_Service)). None of them support the unlock scenario, though...

Stay tuned !
