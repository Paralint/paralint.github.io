---
author: ixe013
comments: false
date: 2009-02-09 15:34:36+00:00
layout: post
link: http://www.paralint.com/blog/2009/02/09/segmentation-example-in-a-captcha/
slug: segmentation-example-in-a-captcha
title: Segmentation example in a CAPTCHA
wordpress_id: 99
categories:
- Other technical
---

From time to time, I come across an application whose designers need - or think they need - a CAPTCHA. I stay convinced that CAPTCHA are to be avoided. This post just goes to show the effect of segmentation on optical character recognition (OCR).  
  
If you read about artificial intelligence and character recognition, you will hear that there are references to segmentation. In short, segmentation is separating the letters from each other, before trying to guess what letters are there.  
  
Segmentation is the "hard" part in solving a text based CAPTCHA, background noise and colors are the easy part. As a rule of thumb, if the letters of your CAPTCHA do not touch each other, your CAPTCHA is weak.  


Here is an example. With a stock build of [ocrad](http://www.gnu.org/software/ocrad/ocrad.html), I have tried to get the text from the same image, with one or two lines over the text.  




<table bgcolor="#ffffff" border="0" class="zeroBorder" width="100%" cellpadding="3" cellspacing="0" id="j3z3" >
<tbody id="wlf87" >
<tr id="wlf88" >

<td width="33%" id="wlf89" >![banane](/blog/images/{{ page.date | date: "%Y-%m-%d" }}-{{ page.slug }}/banane.gif)
</td>

<td width="33%" id="wlf811" >![banane-11](/blog/images/{{ page.date | date: "%Y-%m-%d" }}-{{ page.slug }}/banane-11.gif)  

</td>

<td width="33%" id="wlf814" >![banane-21](/blog/images/{{ page.date | date: "%Y-%m-%d" }}-{{ page.slug }}/banane-21.gif)  

</td>
</tr>
<tr id="wlf817" >

<td width="33%" id="wlf818" >$ ocrad -v banane.pbm  
processing file 'banane.pbm'  
file type is P4  
file size is 175w x 66h  
number of text blocks = 1  
**BANANE**  

</td>

<td width="33%" id="wlf825" >$ ocrad -v banane-1.pbm  
processing file 'banane-1.pbm'  
file type is P4  
file size is 175w x 66h  
number of text blocks = 1  
**_ANE**  

</td>

<td width="33%" id="wlf832" >$ ocrad -v banane-2.pbm  
processing file `banane-2.pbm'  
file type is P4  
file size is 175w x 66h  
number of text blocks = 1  
  

</td>
</tr>
</tbody></table>



The text goes from 100% to 0% percent recognition just by adding two lines ! The word BANANE, then _ANE and after that ... nothing !

This CAPTCHA is by no means robust, and I stay convinced that all forms of CAPTCHA are to be avoided. This example just goes to show the effect of segmentation on optical character recognition (OCR).  
  
After all, artificial intelligence is a field of expertise you can spend your life learning. Just like cryptography, it should not be done by amateurs. But unlike cryptography, an AI challenge has no key. [Microsoft](http://www.websense.com/securitylabs/blog/blog.php?BlogID=171) and [Google](http://www.websense.com/securitylabs/blog/blog.php?BlogID=174)'s CAPTCHA have been broken. Your CAPTCHA will be broken too, it someones takes a shot at it. It is a matter of time, and there is a shorter way than brute force.  
  
If you think you must put a CAPTCHA, start thinking about plan B right away... Using an image based CAPTCHA is not good either (post in French).
