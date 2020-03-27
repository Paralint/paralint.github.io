---
author: ixe013
comments: false
date: 2008-03-22 02:48:25+00:00
layout: post
link: http://www.paralint.com/blog/2008/03/21/reversing-websphere-xor-password-protection/
slug: reversing-websphere-xor-password-protection
title: Reversing WebSphere {xor} password protection
wordpress_id: 50
categories:
- Security
---

IBM WebSphere stores its passwords in files. Everybody does that and it is hard to do otherwise. When I am confronted with the problem, I usually say that the only option you have is what file you want a password in. IBM (in WebSphere) went a little further by applying a hardcoded XOR. Each caracter is XORed with the caracter '_', and the resulting string is encoded in base64. This is not cryptography, it is just enough encoding so that a casual glance at the file will not reveal the password.

If you have access to security.xml and need to know the passwords it contains, compile and run this tool. It supports :



	
  * Encoded passwords on the command line (as many as you like)

	
  * Passwords piped in (default if no arguments are passed)

	
  * With or without the leading {xor}

	
  * It builds with Visual C++ and GNU g++ (tested with mingw32 version only)

	
  * A crude but working parsing so you can pipe the result of a grep, like this :
`grep -i password security.xml | waspass`


You can get the source from my Subversion server with this command :

    
    svn co http://src.paralint.com/spikes/waspass/trunk waspass


Update: Here it is in Python

    
    import base64
    
    #Decode
    xored = 'KD4sPjsyNjE='
    plain = ''.join([chr(ord(c)^ord('_')) for c in base64.b64decode(xored)])
    #Encode
    enocoded = base64.b64encode(''.join([chr(ord(c)^ord('_')) for c in plain]))


Update: With WebSphere v5, you can acheive the same result with IBM's own classes

Eric Haszlakiewicz, of [SwapSimple.com](http://www.swapsimple.com/), found a way to acheive the same result with the server's own Java classes. Here it is :

    
    cd /opt/WebSphere/AppServer/lib
    
    ../java/bin/java -cp securityimpl.jar:iwsorb.jar com.ibm.ws.security.util.PasswordEncoder secret
    ../java/bin/java -cp securityimpl.jar:iwsorb.jar com.ibm.ws.security.util.PasswordDecoder '{xor}LDo8LTor'




Update:The same recipe with WebSphere v6


Thanks to Thomas for this one !

    
    C:\Rational\SDP\6.0\runtimes\base_v6\lib>..\java\bin\java -cp ffdc.jar;bootstrap.jar;emf.jar;securityimpl.jar;iwsorb.jar;ras.jar;wsexception.jar com.ibm.ws.security.util.PasswordDecoder {xor}LDo8LTor
    


<!-- more -->
Now the code, if you can't use WebSphere's classes...

    
    #include <stdio.h>
    #include <string.h>
    
    // get those 2 functions from
    // http://src.paralint.com/spikes/waspass/trunk/waspass/base64.c
    extern "C" int base64_init(void);
    extern "C" int base64_decode(char *d, unsigned dlen, const char *s);
    
    int decode_password(char *encoded_password);
    
    //Cass decode_password, reading from the command line or stdin
    int main(int argc, char* argv[])
    {
    	printf("Reverses WebSphere XOR password encoding.\n");
    	printf("http://www.paralint.com/\n\n");
    
    	base64_init();
    
    	//Should we parse stdin
    	if(argc == 1)
    	{
    		char line[2048];
    		while(!feof(stdin))
    		{
    			fgets(line, sizeof line, stdin);
    			decode_password(line);
    		}
    	}
    	//Or read encoded passwords from the command line ?
    	else for(int i=1; i<argc; ++i)
    	{
    		decode_password(argv[i]);
    	}
    
    	return 0;
    }
    
    //Takes an encoded password like KzY4Oi0= and outputs the original password
    //Supports minimal parsing: a password is the text between } and " (quote)
    //either are optionnal and will be replaced by begining or end of line if
    //missing
    int decode_password(char *encoded_password)
    {
    	char *p;
    	char encoded[1024];
    
    	//naive remove the {xor} flag if present
    	p = strchr(encoded_password, '}');
    	if(p) ++p; else p = encoded_password;
    
    	//naive truncate of the string
    	strtok(p, "\"");
    
    	printf("%s ", p);
    	base64_decode(encoded, sizeof encoded, p);
    	p = encoded;
    
    	//stop at the trailing quote, allowing a brutal pipe from grep
    	while(*p && (*p != '\"'))
    	{
    		putc(*p++ ^ '_', stdout);
    	}
    
    	printf("\n");
    
    	return p - encoded;
    }
