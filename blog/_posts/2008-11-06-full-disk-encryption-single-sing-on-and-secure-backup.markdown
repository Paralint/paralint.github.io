---
author: ixe013
comments: false
date: 2008-11-06 21:22:19+00:00
layout: post
link: http://www.paralint.com/blog/2008/11/06/full-disk-encryption-single-sing-on-and-secure-backup/
slug: full-disk-encryption-single-sing-on-and-secure-backup
title: Full disk encryption single sing-on and secure backup
wordpress_id: 67
categories:
- Cryptography
- Security
---

I have a been a [TrueCrypt](http://www.truecrypt.org/) user since version 4.0. I used to have an half-baked solution of TrueCrypt, EFS with SYSKEY option 2 (password). When full disk encryption was introduced, I finally got a laptop encryption scheme that I like. It features :



	
  * **Strong cryptography**
Thank TrueCrypt for 256 bits AES in XTS mode. I think 256 bits is overkill, but 128 is not offered. I don't see any performance hit on my modest, stock Fujitsu E8210 laptop.

	
  * **Need to know (reduced data exposure)**
Data is not available in clear text when I don't need it. In other words, when I work, I have my files, when I play they stay encrypted

	
  * **Easy encrypted backup**
My backups are merely a copy to a file server.

	
  * **Single sign-on to any encrypted volume**
The pre-boot authentication password (or pass phrase, your call) is the only one you'll ever have to enter, and yet, that password is never stored anywhere. Not even in encrypted memory. It's only in your head.

	
  * **Supports encrypted USB drive**
USB drives get the same single sign-on, need to know and backup features. Doesn't matter wheter you use file based or whole volume, although using a file based container allows you to store regular data on any computer, instead of carrying to drives.

	
  * **Platform independent**
Works on all platforms that TrueCrypt supports







All that out of the box. Well... actually there is no box, it is all open source !




<!-- more -->


It does not feature, but could be extended to :



	
  * Plausible deniability

	
  * Two factor authentication to encrypted files (TrueCrypt version 6.1 required)

	
  * Step-up authentication to encrypted files

	
  * Operating system logon integration (stay tuned for that one...)

	
  * Full operating system backup


Here is a simplified view of my setup. A laptop, a usb drive and a simple NAS server (I have a Linksys NAS200, but any remote file share or ftp will do).

[![Full disk encryption single sign-on diagram](/blog/images/{{ page.date | date: "%Y-%m-%d" }}-{{ page.slug }}/tcsso.jpg)](/blog/images/{{ page.date | date: "%Y-%m-%d" }}-{{ page.slug }}/tcsso.jpg)



	
  1. **A keyfile is stored on your encrypted partition. **
I generated a keyfile with cryptographic random noise. Let's call it Entropy.dat. Your pre-boot password and operating system logon will give you access to that key file. It is used to single sing-on to any container. That keyfile is never backed up, excluded it from all your backups.

	
  2. **A volume header of your container (with password authentication) is backed up**
For any file based volume you create, backup a header that has a password authentication, no keyfiles. Write that password behind a picture of yourself with your kids and send it to your mother. It will be on the her fridge if ever you need it.

	
  3. **Backup a rescue disk ISO file**
This is regular TrueCrypt procedure for full disk encryption.


To set yourself up like this, follow these steps :

	
  1. Follow TrueCrypt's guidelines to enable full disk encryption.

	
  2. Create a file based TrueCrypt volume, with a strong password that you will remember or write down.

	
  3. Backup that volume header.

	
  4. Select (or generate) a keyfile.

	
  5. Change the volume password to      (nothing, leave the password field blank)

	
  6. Select the keyfile of step 4 and click Ok


Repeat steps 4-5-6 for each file based container. Copy to that container the files you want to be able on a need to know basis. When you need the files, mount the container. I wrote a batch file that mounts a file based container and shows a popup with my [Notifu utility](/projects/notifu/) (Windows only).

    
    @echo off
    REM Mounts a file based TrueCrypt container and displays a pop-up
    "C:\Program Files\TrueCrypt\TrueCrypt.exe" /v C:\users\your_username\Clients.tc /l X /q /k "%USERPROFILE%\entropy.dat" /m ts
    start "" notifu /m "TrueCrypt drive X was mounted successfully from file Clients.tc" /p "Secure drive mounted" /d 5000 /i "C:\Program Files\TrueCrypt\TrueCrypt.exe"
    start "" /MIN "C:\Program Files\TrueCrypt\TrueCrypt.exe" /q background


The batch is a little different for USB drives.

    
    @echo off
    REM Mounts a file based TrueCrypt container located on a USB drive and displays a pop-up
    setlocal
    REM I use this setup on many machines, and the USB drive is not
    REM always given the same letter...
    if exist f:\mobile.tc set TCFILE=f:\mobile.tc
    if exist e:\mobile.tc set TCFILE=e:\mobile.tc
    start "TrueCrypt" /MIN "C:\Program Files\TrueCrypt\TrueCrypt.exe" /v %TCFILE% /k "%USERPROFILE%\entropy.dat" /l U /a /q /m rm /m ts
    start "" notifu /m "TrueCrypt drive U was mounted successfully from file %TCFILE%" /p "Secure drive mounted" /d 5000 /i "C:\Program Files\TrueCrypt\TrueCrypt.exe"
    start "" /MIN "C:\Program Files\TrueCrypt\TrueCrypt.exe" /q background
    endlocal


Feel free to use it and adapt it to your needs !
