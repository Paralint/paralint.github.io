---
author: ixe013
comments: false
date: 2008-03-18 17:57:35+00:00
layout: post
link: http://www.paralint.com/blog/2008/03/18/asymmetrick/
slug: asymmetrick
title: Asymmetric cryptography mnemonic trick
wordpress_id: 48
categories:
- Cryptography
---

When ever I teach cryptography to beginners, they are confused   with what you can do with the private and public key, in an   asymmetric cryptographic scheme. I start by saying the your   private key never leaves you, no matter what. No exception to the   rules.

To help with the rest, I made this chart.
<table cellpadding="3" cellspacing="0" border="1" id="wzwo" width="100%" >
<tr >
Operation (below) key used (right)
Public key
Private key
</tr>
<tr >

<td >Encryption (done by the sender)
</td>

<td bgcolor="#66cccc" >Encrypt a message for           an individual (that "message" is often a symmetric           key)
</td>

<td bgcolor="#ffcc33" >Generate a digital           signature (encrypt a document hash)
</td>
</tr>
<tr >

<td >Decryption (done by the           receiver)
</td>

<td bgcolor="#ffcc00" >Verify a digital           signature (decrypt a hash of the message)
</td>

<td bgcolor="#66cccc" >Decrypt a message           destined to you (that "message is often a symmetric           key)
</td>
</tr>
</table>
The colors in that chart indicate operations that are related to each other. To put it in words:



	
  * If you use a public key for encryption, you will use your private key for decryption.

	
  * If you use a private key for encryption, you will use a public key for decryption


But most students need some time to reach the asymmetric   cryptography enlightenment. When they do reach it, I have to   convince them that it is not the silver bullet it looks like. I   found that remembering this chart helps them cram study for an   exam.

Hope this helps !
