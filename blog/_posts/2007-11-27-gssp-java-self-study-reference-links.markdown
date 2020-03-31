---
author: ixe013
comments: false
date: 2007-11-27 16:05:59+00:00
layout: post
link: http://www.paralint.com/blog/2007/11/27/gssp-java-self-study-reference-links/
slug: gssp-java-self-study-reference-links
title: GSSP - Java self study reference links
wordpress_id: 23
categories:
- Security
---

This page contains links to useful, free content to prepare to your GSSP certification. I gathered these links as I was preparing for the exam myself.

Start by reading the [exam blueprint here](http://www1.sans-ssi.org/blueprint_files/java_blueprint.pdf). All the links below are shown in hyperlink and in full text, so you can study with only a printed version of this page.


This page is probably not enough to pass the test. You won't get by just by learning everything here by heart. But it will help you get your study started and quickly find the areas you need to focus on.


There are topics I do not cover. It's not because they are not important, it's just that I skipped over the stuff I was most familiar with. It doesn't mean it's not important !

Good luck ...

UPDATE : I passed the exam, I am a certified GIAC Secure Software Programmer for Java ! That means that I have more letters to put after my name than my name contains ;)

<!-- more -->


## **Task 1 : Input Handling**




### 1.1.1 Input Validation Principles


That one is easy : trust no one. Any data has to go through some verification before it trusted to move along. The close you get to core, the more check should be applied. It's better to do a little verification, each adding more trust to the input, all the way down to the core of your application (like the database).


## 1.1.2 Input Validation Sources




### 1.1.3 Input Validation Techniques


[Regular expressions](http://java.sun.com/docs/books/tutorial/essential/regex/char_classes.html) : [http://java.sun.com/docs/books/tutorial/essential/regex/char_classes.html](http://java.sun.com/docs/books/tutorial/essential/regex/char_classes.html)

Quantifiers apply to single characters, character classes (including predefined ones) and capturing groups.
<table cellpadding="3" cellspacing="0" border="1" width="100%" >
<tr border="1" >
Quantifier
Example
Will match
Will not match
</tr>
<tr >

<td >* means any number
</td>

<td >z*
</td>

<td >"foo"
"gssp"
"zoo"
""
</td>

<td >
</td>
</tr>
<tr >

<td >+ means one or more
</td>

<td >z+
</td>

<td >"zoo"
"snazzy"
</td>

<td >"zoo"
"lazy"
"foo"
</td>
</tr>
<tr >

<td >? means 0 or 1
</td>

<td >z?
</td>

<td >"zoo"
"lazy"
""
</td>

<td >"foo"
</td>
</tr>
</table>


## **Task 2 : Authentication & Session Management**




### 1.2.1 When to Authenticate




### 1.2.2 Authentication Protection




### 1.2.3 Session Protection




### 1.2.4 Authentication Techniques


JAAS : Try the book Core Security Patterns, from page 197

JAAS Defines a Subject (javax.security.auth.Subject) that is a container for one or more Principals (java.security.Principal). A principal is a name that is bound to a Subject. JAAS handles authentication. It can also handle authorization.

JAAS is three things :



	
  1. Common classes (Subject and Principal),

	
  2. Authentication classes (LoginContext, LoginModule, Configuration and CallbackHandler),

	
  3. Authorization classes (Policy and a few others).


A client will instanciate a LoginContext. That LoginContext will read the configuration and instanciate the required LoginModules (more on that next). Each LoginModule will create a set of callbacks used to interact with the user (through the CallbackHandler). Here is a collaboration diagram that helped me. It shows a configuration with only one username-password logon module.

![JAAS collaboration diagram](/blog/images/{{ page.date | date: "%Y-%m-%d" }}-{{ page.slug }}/jaas-collaboration-diagram1.png)

The method Subject.doAs will associate the Subject and the Action with the current access control context. Subject.doAsPriviledge will associate the Subject and the Action to a specific access control context.

The LoginContext enforces the required, requisite, sufficient and optional flags assigned to each LoginModule in the configuration file. It will process them according to their authentication flags, as describe in this table :
<table cellpadding="2" cellspacing="0" border="1" width="100%" >
<tr >
Authentication flag
Must succeed
Login process will continue
</tr>
<tr >

<td >required
</td>

<td >yes
</td>

<td >always
</td>
</tr>
<tr >

<td >requisite
</td>

<td >yes
</td>

<td >only if successful
</td>
</tr>
<tr >

<td >sufficient
</td>

<td >no
</td>

<td >only if failed
</td>
</tr>
<tr >

<td >optional
</td>

<td >no
</td>

<td >always
</td>
</tr>
</table>


### 1.2.5 Authentication responsibilities


I wasn't sure where to put the web.xml stuff. It's on the exam, since this section talks about maximum session length, it thought this would be the place for it. Here is a hierarchy of the security related tags in web.xml

[WEB.XML](http://edocs.bea.com/wls/docs61/webapp/web_xml.html) : [http://edocs.bea.com/wls/docs61/webapp/web_xml.html](http://edocs.bea.com/wls/docs61/webapp/web_xml.html)

security-constraint
pattern
method
auth-constraint
role
user-data-constraint
NONE, CONFIDENTIAL or INTEGRAL. (None is nothing, the other two will get you SSL)
login-config
auth-method (BASIC, FORM, CLIENT-CERT)
real-name (optionnal)
form-login-config
security-role (just a name a description)

What's wrong with BASIC authentication ?



	
  * The password is in the clear (actually, base 64 encoded)

	
  * There is no way to log out !


Even if you did log out the user by destroying its session, it would be recreated at the next call because the browser caches the credentials and sends them with every request.

By default, the session never times out. You need to set it (in seconds) in the session-config/session-timeout tag

Always specify CONFIDENTIAL or INTEGRAL if your auth-method is CLIENT-CERT. The latter will get you HTTPS for authentication only. But if you HTTP server can serve content to/from your application and you forgot to set this, you might get redirected to a regular HTTP session where your session cookie could be stolen.


## **Task 3 : Access Control (Authorization)**




### 1.3.1 Restricting Access To Resources




### 1.3.2 Restricting Access To Functions




### 1.3.3 Declarative Access Control


A full fledge J2EE application will have these configuration files for security : application.xml, web.xml, ejb-jar.xml and ra.xml. An EAR file can contain any number of those, and each can be package on its own. Most (if not all) implementation will have another xml file alongside the standard files to do implementation specific stuff.

![Java J2EE declarative security configuration files](/blog/images/{{ page.date | date: "%Y-%m-%d" }}-{{ page.slug }}/declarative-security.png)


### 1.3.4 Programmatic Access Control


There are two things you can do and to ways to get them depending on the context (web application or enterprise java bean) you are in. This table sums it up :
<table cellpadding="2" cellspacing="0" border="1" width="100%" >
<tr >
What you can do
Web Module
EJB
</tr>
<tr >

<td >Get the identity of the caller
</td>

<td >HttpServletRequest.getUserPrincipal()
</td>

<td >EJBContext.getCallerPrincipal()
</td>
</tr>
<tr >

<td >Check authorization
</td>

<td >HttpServletRequest.isUserInRole(String)
</td>

<td >EJBContext.isCallerInRole(String)
</td>
</tr>
</table>
Remember that when you get to those methods, the Subject has already been authenticated (by whatever means). So get the identity of the caller has nothing to do with authentication. At this point, it's identification.


### 1.3.5 JAAS


It's already covered in section 1.2.4 !


## **Task 4 : Java types & JVM Management**




### 1.4.1 java.lang.String


[String literals](http://www.janeg.ca/scjp/lang/strLiteral.html) : [http://www.janeg.ca/scjp/lang/strLiteral.html](http://www.janeg.ca/scjp/lang/strLiteral.html)


### 1.4.2 Integer and Double overflow


[Integer overflows](http://www.phrack.org/archives/60/p60-0x0a.txt) : [http://www.phrack.org/archives/60/p60-0x0a.txt](http://www.phrack.org/archives/60/p60-0x0a.txt)
[Double overflows](http://www.javacoffeebreak.com/books/extracts/javanotesv3/c9/s1.html) : [http://www.javacoffeebreak.com/books/extracts/javanotesv3/c9/s1.html](http://www.javacoffeebreak.com/books/extracts/javanotesv3/c9/s1.html) (scroll down, its at the bottom)

Integers wrap to negative values if they are signed, or to 0 if they are unsigned. Having an integer wrap around could cause anything from a array out of bounds exception to allowing a runtime access check to succed.

Java Double does not "wrap around" to negative values. Instead, they are represented by special values that have no numerical equivalent. The values Double.POSITIVE_INFINITY and Double.NEGATIVE_INFINITY represent numbers outside the range of legal values


### 1.4.4 ArrayList vs Vector


I will have to check this one out myself... It boils down to this, but apart from the first item (synchronization), opinions differs.



	
  1. Arraylist is not synchronized while vector is.

	
  2. Arraylist increment it's size by half the initial capacity, Vector increments by the full capacity each time.

	
  3. Arraylist can be seen directly without any iterator while vector requires an iterator to display all it's content. (not very sure).

	
  4. Vector works with a 1.1 JVM.




### 1.4.5 Class security


[Accessibility Modifiers](http://java.sun.com/docs/books/tutorial/java/javaOO/accesscontrol.html) : [http://java.sun.com/docs/books/tutorial/java/javaOO/accesscontrol.html](http://java.sun.com/docs/books/tutorial/java/javaOO/accesscontrol.html)

That's rather easy... And its a duplicate of topic 1.9.1 !

[Serialization](http://www.onjava.com/pub/a/onjava/excerpt/JavaRMI_10/index.html?page=3) : [http://www.onjava.com/pub/a/onjava/excerpt/JavaRMI_10/index.html?page=3](http://www.onjava.com/pub/a/onjava/excerpt/JavaRMI_10/index.html?page=3), [http://java.sun.com/developer/technicalArticles/ALT/serialization/](http://java.sun.com/developer/technicalArticles/ALT/serialization/)

It has be done properly. It can be abused to get around a Singleton requirement, clone an object that refuses to be cloned or to get to private data inside a class.

Fields with the transient keyword will not be serialized. But you can control what will be serialized with ObjectStreamField. It takes precedence over transient fields.

[Cloning](http://java.sun.com/javase/6/docs/api/java/lang/Object.html#clone()) : [http://java.sun.com/javase/6/docs/api/java/lang/Object.html#clone()](http://java.sun.com/javase/6/docs/api/java/lang/Object.html#clone())

That's straight from the horse's mouth, as my mother used to say. Clone does only a shallow copy.

[Inner classes](http://www.cs.umd.edu/~pugh/java/SecureInnerClasses.pdf) : [http://www.cs.umd.edu/~pugh/java/SecureInnerClasses.pdf](http://www.cs.umd.edu/~pugh/java/SecureInnerClasses.pdf)

Inner classes are not supported (or understood if you prefer) by the JVM. It was to allow for a JVM 1.0 to work with code that had inner classes. The problem is that inner classes are given package level visibility. Inner classes are just a compile time trick. If you add a malicious class to the package (see 1.9.2), it can call private methods on your inner class.


### 1.4.6 Code Privileges


[Policy file](http://java.sun.com/j2se/1.4.2/docs/guide/security/PolicyFiles.html#FileSyntax) : [http://java.sun.com/j2se/1.4.2/docs/guide/security/PolicyFiles.html#FileSyntax](http://java.sun.com/j2se/1.4.2/docs/guide/security/PolicyFiles.html#FileSyntax)

Before performing a sensitive operation, the `SecurityManager` determines the operation's identity and whether it can be performed in its security context.

The SecurityManager is enable by specifying -Djava.security.manager on the JVM command line (it may be buried deep down in your J2EE application server). It is not enabled by default, except for applets or web start applications.

The class loader is responsible for locating and fetching the class file, consulting the security policy, and defining the class object with the appropriate permissions. There is by default a single system-wide policy file, and a single (optional) user policy file. You always get a union of all the policy, unless you specify a policy file with == (instead of =).

Code security is based on protection domains. A protection domain is the combination of classes, grants and permissions. There are two types of protection domains :



	
  * System domain : files, sockets and everything having a foot in the native world

	
  * Application domain : classes and objects (instances of classes)


The ClassLoader puts classes it loads in a protection domain and it stays there. You cannot change protection domains at runtime.

A SecurityManager uses a policy file, that has grant entries based on any combination of codebase, signed by or Principal. If there are multiple principals on a single grant entry, a Subject must have every principal.


## **Task 5 : Application Faults & Logging**




### 1.5.1 Exception Handling




### 1.5.2 Logging




### 1.5.3 Configuration of Error Handling


[Error pages](http://edocs.bea.com/wls/docs61/webapp/web_xml.html#1017571) : [http://edocs.bea.com/wls/docs61/webapp/web_xml.html#1017571](http://edocs.bea.com/wls/docs61/webapp/web_xml.html#1017571)

In WEB-INF\web.xml file, you can configure what page is used when an error occurs, like 404 (not found) or 500 (Server Error) or a specific Exception bubbles back up to the web container. The location of the page is relative to the root of your web application.

```
   <error-page>
      <error-code>404</error-code>
      <location>/404error.html</location>
   </error-page>
   <error-page>
      <error-code>500</error-code>
      <location>/500error.html</location>
   </error-page>
   <error-page>
      <exception-type>javax.servlet.ServletException</exception-type>
      <location>/ExceptionHandler.jsp</location>
   </error-page>

```



## **Task 6 : Encryption Services**




### 1.6.1 Communications Encryption


You configure SSL (JSSE) using a keystore. Both the client and the server have their own, independent keystores (assuming their both written in Java). They also have another keystore, called a truststore, that holds the public key certificates of any number of trusted root certificate authorities (root CA). Messing up the truststore will get you the dreaded pop-up saying the certificate cannot be trusted.

A server-side keystore (the end that waits for a SSL connection, like a HTTPS web server would). :



	
  * Has at least one private key, protected by a password

	
  * The password can be in a configuration file or entered at startup (that's implementation dependent)

	
  * The private key is used to issue a server certificate, from which a certificate signing request is generated and sent to a root CA the _client trusts_ (although you will much likely trust it yourself).

	
  * The CA signs the server certificate and it is installed in the keystore


A client-side keystore is used to hold a private key that could be used to authenticate with a server. It is optional and rarely used for end users. For this to work, you must have :

	
  * In the client keystore a certificate corresponding to the private key you hold.

	
  * That certificate issued by a root CA the _server trusts_ (although you will much likely trust it yourself).

	
  * The password (or other authentication) to decrypt that private key you hold.


About truststores : Java comes with a keystore called cacerts that contains public key certificates of all the major root CA in the world, and then some. So having a client root CA trusted by the server (or vice-versa) is rather easy if you're willing to spend the money. If not, you can [set up your own root CA](http://sial.org/howto/openssl/ca/) and add that CA's certificate to the truststore at both end.

The following table summarize what goes on at both ends of an SSL connection programmatically, with or without a client certificate.
<table cellpadding="2" cellspacing="0" border="1" width="100%" >
<tr >
Client
Server
</tr>
<tr >

<td >Register providers
</td>

<td >Register providers
</td>
</tr>
<tr >

<td >Create a SocketFactory (this is where you set a proxy, if needed)
</td>

<td >Create a SocketFactory (and set your client certificate requirement if any)
</td>
</tr>
<tr >

<td >Create socket
</td>

<td >Accept
</td>
</tr>
<tr >

<td >Create or get input and output streams
</td>

<td >Create or get input and output streams
</td>
</tr>
<tr >

<td >Close everything
</td>

<td >Close everything
</td>
</tr>
</table>


## 1.6.2 Encryption of Data at Rest


There is two API, because of extensibility and export regulation. Every Java installation has the Java Cryptography Architecture (JCA). Not to be confused with the Java Connector Architecture, also referred to as JCA. The Java Cryptography Architecture contains classes able to do the following :



	
  * Message Digest algorithm (MD5 and SHA-1)

	
  * Key pair generation (for DSA only)

	
  * Key store manipulation (JKS only)

	
  * Digital signature (again, for DSA only)

	
  * Cryptographic strength random number generator


Pretty much everything else is in JCE. You can have has many providers as you like, of any algorithm you like that can perform these cryptographic operations :

	
  * Symmetric key generation

	
  * Symmetric key encryption (from weak like DES to unlimited AES-256 and up)

	
  * Password based encryption (PBE or sometimes PBKDF2)

	
  * Message authentication codes (MAC, a symmetric key signature)

	
  * PKCS11 support (hardware cryptographic device such as smart cards)


The graphic below is taken from the book [Core Security Patterns](http://www.coresecuritypatterns.com/).

![Java crypto relations](/blog/images/{{ page.date | date: "%Y-%m-%d" }}-{{ page.slug }}/java-crypto-relations.png)

You can serialize an object in an encrypted form, you create a CipherOutputStream and use it with a ObjectOutputStream.


## Task 7 : Concurrency and Threading




### 1.7.1 Race Conditions




### 1.7.2 Singletons & Shared resources


[Singleton pattern](http://www.javacoffeebreak.com/articles/designpatterns/index.html) : [http://www.javacoffeebreak.com/articles/designpatterns/index.html](http://www.javacoffeebreak.com/articles/designpatterns/index.html)

A singleton is any regular class that has these particular implementation features



	
  * It has an empty, private constructor

	
  * It has a public static synchronized method that returns the unique instance of the object (say MyClass.getSingleInstance())

	
  * It creates an instance of itself on the first call to the accessor method (MyClass.getSingleInstance())

	
  * Implements a clone() method that always throw a CloneNotSupportedException


But you can bypass a singleton class by serializing the object to a temporary stream and back to another instance of it. See 1.4.5


## Task 8 : Connection Patterns




### 1.8.1 Parameterized Queries/Prepared Statements




### 1.8.2 Output Encoding


[HTML Output Encoding](http://www.owasp.org/index.php/How_to_perform_HTML_entity_encoding_in_Java) : [http://www.owasp.org/index.php/How_to_perform_HTML_entity_encoding_in_Java](http://www.owasp.org/index.php/How_to_perform_HTML_entity_encoding_in_Java)

There is no standard way to do HTML output encoding. The characters to watch out for are < > / '  &, but OWASP suggest to encode everything that is not a a letter or digit.

Use output encoding when you are sending back data you got from a client, even if you validated it on its way in. There were double decode bugs in the past...


## **Task 9 : Miscellaneous**




## 1.9.1 Class/Package/Method Access Modifiers


[Accessibility Modifiers](http://java.sun.com/docs/books/tutorial/java/javaOO/accesscontrol.html) : [http://java.sun.com/docs/books/tutorial/java/javaOO/accesscontrol.html](http://java.sun.com/docs/books/tutorial/java/javaOO/accesscontrol.html)

Here is the duplicate of topic 1.4.5 !


#### 1.9.2 Class File Protection


[JAR sealing](http://java.sun.com/developer/JDCTechTips/2001/tt0130.html) : [http://java.sun.com/developer/JDCTechTips/2001/tt0130.htmlg](http://java.sun.com/developer/JDCTechTips/2001/tt0130.htmlg)

JAR sealing prevents classes defined anywhere other than the original JAR to see public methods of that original JAR. Someone could create a class with the same package name as yours and call your public methods and break things.



	
  * Sealing is off by default (packages are not secured by default)

	
  * You can apply RuntimePermissions to a policy file with accessClassInPackage and defineClassInPackage, but default classloaders don't enforce them !

	
  * In java.security (located in ${JAVA_HOME}/jre/lib/security) you can protect classes with package.access and package.definition

	
  * Class visibility is package by default

	
  * java.* has its seal hardcoded in the classloader.

	
  * You put the Sealed: true directive in the manifest (META-INF/MANIFEST.MF)




### 1.9.3 JAVA EE Filters


[Writing servlet filters](http://javaboutique.internet.com/tutorials/Servlet_Filters/index.html) : [http://javaboutique.internet.com/tutorials/Servlet_Filters/index.html](http://javaboutique.internet.com/tutorials/Servlet_Filters/index.html)
[The essentials of Filters](http://java.sun.com/products/servlet/Filters.html) : [http://java.sun.com/products/servlet/Filters.html ](http://java.sun.com/products/servlet/Filters.html)

A J2EE Filter is applied to a servlet, not an Enterprise Java Bean (EJB).



	
  * They are configured in web.xml.

	
  * They can read, modify or block requests and responses to/from a servlet.

	
  * They cannont filter arbiratry request (for example, a request to an image).

	
  * Any number of filters can be applied to any number of servlets

	
  * Security roles do not apply to Filters


