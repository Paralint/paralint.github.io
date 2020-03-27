---
author: ixe013
comments: false
date: 2007-01-16 19:16:24+00:00
layout: post
link: http://www.paralint.com/blog/2007/01/16/change-paros-proxy-mitm-ssl-certificate/
slug: change-paros-proxy-mitm-ssl-certificate
title: Change Paros Proxy MITM SSL certificate
wordpress_id: 9
categories:
- Security
---

Don't you just love man-in-the-middle (MITM) HTTPS proxies ? I use [Burp proxy](http://www.portswigger.net/proxy/) a lot, it does man-in-the-middle and gzip.

But I have an assignment where the client configuration cannot be changed. The client rejects certificates from non-trusted CA and I cannot add Portswigger's certificate to the trusted roots. I went looking for an open source HTTPS mitm proxy, hoping I could recompile it to use a trusted certificate that I have in hand.

[Paros proxy](http://www.parosproxy.org/) fits the bill. Here is how you can change Paros proxy's man-in-the-middle certificate for your own using keytool, openssl, and jar.

<!-- more -->First of all, Paros certificate is the file ./resource/paroskey. It is a JKS keystore and it's password is "!@#$%^&*()", without the quotes (the password is in the source code, I am not [revealing any secrets](http://www.google.com/codesearch?q=+%22!%40%23%24%25%5E%26*()%22+show:1nVTaHJQG2I:3qN8LO4APgY:RwTWy1IW3Kc&sa=N&cd=5&ct=rc&cs_p=http://gentoo.osuosl.org/distfiles/paros-3.2.9-src.zip&cs_f=paros/src/org/parosproxy/paros/network/SSLConnector.java#a0)). Extract it with this command :

    
    jar xvf paros.jar resource/paroskey


You can view the certificate using this command (you might have to escape some characters for it to work on your platform) :

    
    keytool -list -v -keystore resource/paroskey -storepass "!@#$%^&*()"




If something goes wrong, you can type that command and compare the original and the new certificate. Run it from after each keytool operations if you want to follow what is going on in there.


To keep it as simple as possible, I will simply replace the paroskey file with my own certificate, using the same alias and password that is hardcoded. It will allow you to do the same without the need to use the java SDK. Ok, maybe just keytool.

Now, you have to get your hand on a certificate. I suggest using your own certificate authority (CA) to start and play with the proxy right away. [This page](http://sial.org/howto/openssl/ca/) is perfect for this. It even works well on Windows with cygwin. A better setup would be to have a test CA in your company or lab that would sign your certificates. I also packaged the [two sample certificate files](http://www.paralint.com/blog/wp-content/uploads/2007/06/parosmitm.zip) I refer two in this article in a zip file.

So if you have your CA public key in a file named ca-cert.pem, copy it to the local directory and import it in your brand new keystore with this command :

    
    keytool -import -trustcacerts -alias "my-ca" -file ca-cert.pem -keystore resource/paroskey -noprompt -storepass "!@#$%^&*()"


Now you must choose the hostname of your proxy. It doesn't have to match any DNS record, but if you want your setup to be warning-free, make them match. I choose mitm.paralint.com, and I delete the old certificate and generate a new private key with those two commands :

    
    keytool -delete -alias paros -keystore resource/paroskey -storepass "!@#$%^&*()"



    
    keytool -genkey -keyalg RSA -alias paros -keystore resource/paroskey -storepass "!@#$%^&*()" -keypass "!@#$%^&*()" -dname "CN=mitm.paralint.com" -validity 720




Adjust the validity period to your needs, but do not change the alias or passwords. Now create a certificate signing request with this command :



    
    keytool -certreq -v -alias paros -keystore resource/paroskey -storepass "!@#$%^&*()" -file mitm.paralint.com.csr




Hand over the mitm.paralint.com.csr file to your CA. If you are using the CA suggested above, initialize it with make init, copy the csr file in that same directory as the makefile and type make sign.




Your CA will send you back a signed certificate (mitm.paralint.com.cert). It must be in DER format to be imported in the JKS keystore. If it is not (like if you are using the CA mentionned above), type this command to convert it :



    
    openssl x509 -in mitm.paralint.com.cert -out mitm.paralint.com.der -outform DER




Now import that DER encoded certificate in a JKS keystore with this keytool command :



    
    keytool -import -v -alias paros -file mitm.paralint.com.der -keystore resource/paroskey -storepass "!@#$%^&*()" -storetype JKS




All this work has got you a certificate with a hostname you selected, in a keystore that can be used as a drop-in replacement for Paros built-in hardcoded keystore. Use your favorite tool for this or keep reading if all you have is the jar utility.




Replace the old keystore with the new one in paros.jar with this command (backup paros.jar before, just in case…) :



    
    jar uvf paros.jar resource\paroskey




That’s all there is to it ! Your Paros proxy will now show mitm.paralint.com as it’s hostname. Remember that to be completely warning free, you must trust the root CA. If it’s not already there, import the root CA public key certificate in your browser.
