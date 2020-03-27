---
author: ixe013
comments: false
date: 2007-05-19 03:29:15+00:00
layout: post
link: http://www.paralint.com/blog/2007/05/18/making-jetty-listen-to-the-local-interface-only/
slug: making-jetty-listen-to-the-local-interface-only
title: Making Jetty listen to the local interface only
wordpress_id: 11
categories:
- Other technical
---

[I use](/cms.html) Apache's Forrest tool, which uses internally the Jetty engine. I wanted to make Jetty listen to 127.0.0.1 instead of 0.0.0.0, so my computer wouldn't show up in a enterprise port scan.

I had trouble finding the information. I went looking for the link again without any success. So I am posting it here, where I will always be able to find it !

Update 2008-01-25 : You must add this XML to the file $FORREST_HOME/main/webapp/jettyconf.xml

    
    <Call name="addListener">
      <Arg>
        <New class="org.mortbay.http.SocketListener">      <!-- Add this next line -->
          <Set name="Host"><SystemProperty name="jetty.host" default="localhost"/></Set>
          <Set name="Port"><SystemProperty name="jetty.port" default="8888"/></Set>
          <Set name="MinThreads">5</Set>
          <Set name="MaxThreads">100</Set>
          <Set name="MaxIdleTimeMs">30000</Set>
          <Set name="LowResourcePersistTimeMs">5000</Set>
        </New>
      </Arg>
    </Call>


You know its working when you see this in the Forrest window:

    
    21:05:11.864 EVENT  Apache Cocoon 2.2.0-dev is up and ready.
    21:05:11.864 EVENT  Started SocketListener on 127.0.0.1:8888
    21:05:11.864 EVENT  Started org.mortbay.jetty.Server@df8f5e
