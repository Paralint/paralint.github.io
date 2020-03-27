---
author: ixe013
comments: false
date: 2006-07-20 14:18:35+00:00
layout: post
link: http://www.paralint.com/blog/2006/07/20/aspnet-impersonation/
slug: aspnet-impersonation
title: ASP.NET Impersonation
wordpress_id: 6
categories:
- Security
---

I was looking for a table that showed how client authentication, server authentication and impersonate flag work together. I found many good examples and tutorials explaining how to make any combinaison work, but not a quick reference table.

So I made one. I tested on a Windows Server 2003 with ASP.NET version 2.0. I used wfecth as client to be sure of what was going on. Not every configuration makes sense in real life, but I included it for completeness. HTH !
<table cellpadding="0" cellspacing="0" border="1" >
<tr >

<td >Client sends creds
</td>

<td >Server require creds
</td>

<td >Impersonate
</td>

<td >Result
</td>
</tr>
<tr >

<td >don't care
</td>

<td >No
</td>

<td >false
</td>

<td >NETWORK_SERVICE
</td>
</tr>
<tr >

<td >No
</td>

<td >Yes
</td>

<td >don't care
</td>

<td >401 Unauthorized
</td>
</tr>
<tr >

<td >Yes
</td>

<td >Yes
</td>

<td >False
</td>

<td >NETWORK_SERVICE
</td>
</tr>
<tr >

<td >don't care
</td>

<td >No
</td>

<td >True
</td>

<td >IUSR_MACHINENAME
</td>
</tr>
<tr >

<td >Yes
</td>

<td >Yes
</td>

<td >True
</td>

<td >Domain\User
</td>
</tr>
</table>
ps : Actually, NETWORK_SERVICE is the account the application pool is running under.
