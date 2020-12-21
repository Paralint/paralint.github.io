---
layout: page
title: Get the source code
---


# Download the source code
The source code for this project is hosted in a Subversion repository. It is written in C++ and built with Visual C++ 2013 or later.

You can get the source code in different ways:
 - Check out the sources with your Subversion client `svn checkout http://src.paralint.com/notifu/trunk/ Notifu`
 - Download [zipped sources](/projects/notifu/dl/notifu_src.zip)

# API used

There are two ways that I know of that will let you display a yellow pop-up ballon. 
Whether you should use one or the other depends on the application you are using. 
Notifu is written in C++ but I always keep reuse in mind. The code is small and 
porting to your language should not be difficult.

# IUserNotification or Shell_NotifyIcon ?

That's the first question I wanted to answer when I started this project. The answer is simple. If you have a message loop, you are probably better off with Shell_NotifyIcon. If your application is command line or somewhat capabale of blocking, the IUserNotification interface should be used.

I will not talk about Shell_NotifyIcon, there are plenty of examples available for every language, and then some. Suffice to say that IUserNotification gives you more control on the pop-up. Pretty much all of the features of IUserNotification are available in Notifu's command line.

One thing that IUserNotification provides is a clean way for an application to close the popup ballon without any user intervention.
You must implement IQueryContinue with your closing logic. This is how Notifu handles timeouts and replacing a popup with another.

Windows Vista introduced a new interface, IUserNotification2. It has the same signature as the original Windows XP interface, except for the Show method. That interface allows you to specify a callback that will be called when a user clicks on the the tray icon.

For some reason, the interface was duplicated, instead of doing a QueryInterface on IUserNotification for IUserNotification2 that would have a ShowEx method.

# How is the source organized

Five files can be reused directly in you project : NotifyUser.cpp, it's header NotifyUser.h and NOTIFU_PARAM.h provide the small wrapper for the API, while QueryContinue.cpp and it's header QueryContinue.h provide a sample implementation of a IQueryContinue callback. Since IUserNotification is a COM interface, you must also call CoInitialize or CoInitializeEx before calling NotifyUser. Just fill a NOTIFU_PARAM structure and call NotifyUser.

To start, just add those five files to your project and build it. It should work as is.

Notifu has no dependencies on anything, but you might need the platform SDK.
If you need a feature that Notifu doesn't provide, [ask me for it](guillaume@paralint.com)

# Compiled binary

If you haven't already, you can take a look [at the documentation](index.html) and get the [compiled binary](download.html).
