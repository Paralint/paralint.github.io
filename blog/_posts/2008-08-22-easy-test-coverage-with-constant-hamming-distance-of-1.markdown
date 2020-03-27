---
author: ixe013
comments: false
date: 2008-08-22 03:47:19+00:00
layout: post
link: http://www.paralint.com/blog/2008/08/21/easy-test-coverage-with-constant-hamming-distance-of-1/
slug: easy-test-coverage-with-constant-hamming-distance-of-1
title: Easy test coverage with constant Hamming distance of 1
wordpress_id: 61
categories:
- Math
- Other technical
---

I left a terrible bug in version 1.3 of my [replacement Gina](http://www.paralint.com/projects/aucun/). I didn't want to miss any test case this time, so I wrote a batch file that tests every one of them. That batch file adds a user to a group and a group to the registry. There are two possible groups in the registry, and the user can be a member of either two groups, making 2^(2+2) possibilities, 16 use cases.

After a few lines in, I realized that I would be less work to order the tests in a way that would minimize the change to the configuration between any two tests. In other words, when a n+1 test case required a change to the registry, then the user group membership should not change, and vice-versa. That would also make it easy to investigate a failed test, because only one thing would change between any two tests.

Then it hit me.

Well actually, I had to stop and think for a while. Kind of like my mind restoring a dusty old tape archive... I remembered that mathematician Richard Hamming had a [code for that](http://en.wikipedia.org/wiki/Hamming_distance). It's a numbering scheme where only 1 bit changes between any two numbers. The number of bits that change is the Hamming distance between two numbers. Using four information bits to represent each possible use case, I came up with the following table. The first two rows (MSB, in blue) are user membership to a group, and the two last rows (LSB, in green) is the presence of that group in the registry. Ordering my tests that way gave me a constant Hamming distance of 1.
<table cellpadding="2" cellspacing="0" border="1" width="100%" >
<tbody >
<tr >

<td width="23" align="middle" valign="top" >**Decimal value**
</td>

<td width="23" align="middle" valign="top" >**0**
</td>

<td width="23" align="middle" valign="top" >**1**
</td>

<td width="23" align="middle" valign="top" >**3**
</td>

<td width="23" align="middle" valign="top" >**2**
</td>

<td width="23" align="middle" valign="top" >**6**
</td>

<td width="23" align="middle" valign="top" >**7**
</td>

<td width="23" align="middle" valign="top" >**5**
</td>

<td width="23" align="middle" valign="top" >**4**
</td>

<td width="23" align="middle" valign="top" >**12**
</td>

<td width="23" align="middle" valign="top" >**13**
</td>

<td width="23" align="middle" valign="top" >**15**
</td>

<td width="23" align="middle" valign="top" >**14**
</td>

<td width="23" align="middle" valign="top" >**10**
</td>

<td width="23" align="middle" valign="top" >**11**
</td>

<td width="23" align="middle" valign="top" >**9**
</td>

<td width="23" align="middle" valign="top" >**8**
</td>
</tr>
<tr style="background-color: #B7C9E3;" >

<td width="23" align="middle" valign="top" >Unlock
</td>

<td width="23" align="middle" valign="top" >0
</td>

<td width="23" align="middle" valign="top" >0
</td>

<td width="23" align="middle" valign="top" >0
</td>

<td width="23" align="middle" valign="top" >0
</td>

<td width="23" align="middle" valign="top" >0
</td>

<td width="23" align="middle" valign="top" >0
</td>

<td width="23" align="middle" valign="top" >0
</td>

<td width="23" align="middle" valign="top" >0
</td>

<td width="23" align="middle" valign="top" >1
</td>

<td width="23" align="middle" valign="top" >1
</td>

<td width="23" align="middle" valign="top" >**1**
</td>

<td width="23" align="middle" valign="top" >**1**
</td>

<td width="23" align="middle" valign="top" >**1**
</td>

<td width="23" align="middle" valign="top" >**1**
</td>

<td width="23" align="middle" valign="top" >1
</td>

<td width="23" align="middle" valign="top" >1
</td>
</tr>
<tr style="background-color: #B7C9E3;" >

<td width="23" align="middle" valign="top" >Logoff
</td>

<td width="23" align="middle" valign="top" >0
</td>

<td width="23" align="middle" valign="top" >0
</td>

<td width="23" align="middle" valign="top" >0
</td>

<td width="23" align="middle" valign="top" >0
</td>

<td width="23" align="middle" valign="top" >1
</td>

<td width="23" align="middle" valign="top" >**1**
</td>

<td width="23" align="middle" valign="top" >**1**
</td>

<td width="23" align="middle" valign="top" >1
</td>

<td width="23" align="middle" valign="top" >1
</td>

<td width="23" align="middle" valign="top" >**1**
</td>

<td width="23" align="middle" valign="top" >1
</td>

<td width="23" align="middle" valign="top" >1
</td>

<td width="23" align="middle" valign="top" >0
</td>

<td width="23" align="middle" valign="top" >0
</td>

<td width="23" align="middle" valign="top" >0
</td>

<td width="23" align="middle" valign="top" >0
</td>
</tr>
<tr style="background-color: #c6e88c;" >

<td width="23" align="middle" valign="top" >Unlock
</td>

<td width="23" align="middle" valign="top" >0
</td>

<td width="23" align="middle" valign="top" >0
</td>

<td width="23" align="middle" valign="top" >1
</td>

<td width="23" align="middle" valign="top" >1
</td>

<td width="23" align="middle" valign="top" >1
</td>

<td width="23" align="middle" valign="top" >1
</td>

<td width="23" align="middle" valign="top" >0
</td>

<td width="23" align="middle" valign="top" >0
</td>

<td width="23" align="middle" valign="top" >0
</td>

<td width="23" align="middle" valign="top" >0
</td>

<td width="23" align="middle" valign="top" >**1**
</td>

<td width="23" align="middle" valign="top" >**1**
</td>

<td width="23" align="middle" valign="top" >**1**
</td>

<td width="23" align="middle" valign="top" >**1**
</td>

<td width="23" align="middle" valign="top" >0
</td>

<td width="23" align="middle" valign="top" >0
</td>
</tr>
<tr style="background-color: #c6e88c;" >

<td width="23" align="middle" valign="top" >Logoff
</td>

<td width="23" align="middle" valign="top" >0
</td>

<td width="23" align="middle" valign="top" >1
</td>

<td width="23" align="middle" valign="top" >1
</td>

<td width="23" align="middle" valign="top" >0
</td>

<td width="23" align="middle" valign="top" >0
</td>

<td width="23" align="middle" valign="top" >**1**
</td>

<td width="23" align="middle" valign="top" >**1**
</td>

<td width="23" align="middle" valign="top" >0
</td>

<td width="23" align="middle" valign="top" >0
</td>

<td width="23" align="middle" valign="top" >**1**
</td>

<td width="23" align="middle" valign="top" >1
</td>

<td width="23" align="middle" valign="top" >0
</td>

<td width="23" align="middle" valign="top" >0
</td>

<td width="23" align="middle" valign="top" >1
</td>

<td width="23" align="middle" valign="top" >1
</td>

<td width="23" align="middle" valign="top" >0
</td>
</tr>
<tr >

<td width="23" valign="top" >Expected test result
</td>

<td width="23" valign="top" >Gina
</td>

<td width="23" valign="top" >Gina
</td>

<td width="23" valign="top" >Gina
</td>

<td width="23" valign="top" >Gina
</td>

<td width="23" valign="top" >Gina
</td>

<td width="23" valign="top" >**Force logoff**
</td>

<td width="23" valign="top" >**Force logoff**
</td>

<td width="23" valign="top" >Gina
</td>

<td width="23" valign="top" >Gina
</td>

<td width="23" valign="top" >**Force logoff**
</td>

<td width="23" valign="top" >**Unlock**
</td>

<td width="23" valign="top" >**Unlock**
</td>

<td width="23" valign="top" >**Unlock**
</td>

<td width="23" valign="top" >**Unlock**
</td>

<td width="23" valign="top" >Gina
</td>

<td width="23" valign="top" >Gina
</td>
</tr>
</tbody></table>
The only drawback to this is that all the typing I saved writing my [test batch file](http://src.paralint.com/aucun/branches/force-logoff-bug/tests.cmd), I wasted on this blog post !
