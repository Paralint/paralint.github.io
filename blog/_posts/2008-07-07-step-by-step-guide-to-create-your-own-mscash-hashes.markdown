---
author: ixe013
comments: false
date: 2008-07-07 04:49:37+00:00
layout: post
link: http://www.paralint.com/blog/2008/07/06/step-by-step-guide-to-create-your-own-mscash-hashes/
slug: step-by-step-guide-to-create-your-own-mscash-hashes
title: Step by step guide to create your own MSCASH hashes
wordpress_id: 54
categories:
- Cryptography
- Security
- Windows
---

I wanted to test the relative strength of a password policy. I wanted to run a password cracking tool over different passwords, from a dictionary based password (like Banana42) to a random one (generated with [Password Safe](http://passwordsafe.sourceforge.net/)). Creating users setting passwords and running different password extraction tools was a lot of trouble.

 

I found a detailed [explanation of the MSCASH format](http://www.securiteam.com/tools/5JP0I2KFPA.html). Here is how you make your own MSCASH hashes to do close to reality benchmarks of your favourite password cracking tool.


<!-- more -->
  

The format is MD4(MD4(password) + username). password and username are in Unicode. In the explanation linked above, we have the classical "user" and "password" combination. Using notepad, type your password. Save the file using Unicode format. The first two bytes of the file will be FF and EF, a flag called the byte order mark (BOM). Delete them using [an hexadecimal editor](http://www.mh-nexus.de/hxd/). It should look like this :

 
    
    Offset(h) 00 01 02 03 04 05 06 07 08 09 0A 0B 0C 0D 0E 0F
    00000000  70 00 61 00 73 00 73 00 77 00 6F 00 72 00 64 00  p.a.s.s.w.o.r.d.





Now calculate the first hash with openssl, with a binary output :








    
    openssl dgst -md4 -binary password.unicode.txt > md4.password









Type and save your user name in Unicode format, remove the BOM, and concatenate the Unicode user name to the first hash.








    
    copy /b md4.password + user.unicode.txt md4.password.user









The file should look like this (the first 16 bytes is the md4 hash of the password) :




    
    Offset(h) 00 01 02 03 04 05 06 07 08 09 0A 0B 0C 0D 0E 0F
    00000000  88 46 F7 EA EE 8F B1 17 AD 06 BD D8 30 B7 58 6C  ˆF÷ei.±...½Ø0*Xl
    00000010  75 00 73 00 65 00 72 00                          u.s.e.r.





Now just hash that last file, again with openssl :








    
    openssl dgst -md4 md4.password.user
    MD4(md4.password.user)= 2d9f0b052932ad18b87f315641921cda









You can now use that MSCASH hash for your benchmarks. I hope you find it usefull. I might write a program in C to automate this, If I see good traffic on this post. Spread the word !
