---
layout: default
title: General Windows Application Exception Handling in C#
date: '2013-06-14T21:16:00.000-03:00'
author: Mohammed Fayed
tags:
- C#
modified_time: '2017-07-16T03:16:22.828-03:00'
blogger_id: tag:blogger.com,1999:blog-3008009747231704115.post-7788981114153076440
blogger_orig_url: https://www.fayed.org/2013/06/general-windows-application-exception.html
---

when you use C# to build Windows Forms Application you come to a point when you need a general Exception handing for both you UI and any thread in the application ... its easy just add the following to your Main method in the Program class

```csharp
// Add the event handler for handling UI thread exceptions to the event.
Application.ThreadException += new ThreadExceptionEventHandler(UIThreadException);
 
// Set the unhandled exception mode to force all Windows Forms errors to go through
// our handler.
Application.SetUnhandledExceptionMode(UnhandledExceptionMode.CatchException);
 
// Add the event handler for handling non-UI thread exceptions to the event.
AppDomain.CurrentDomain.UnhandledException += new UnhandledExceptionEventHandler(UnhandledException);

```

and make sure you add the following methods to the Program Class

```csharp
private static void UIThreadException(object sender, ThreadExceptionEventArgs t)
{
    // Handel your Exception here
    MessageBox.Show(t.Exception.Message, "error");
}
 
private static void UnhandledException(object sender, UnhandledExceptionEventArgs e)
{
    // Handel your Exception here
    var ex = e.ExceptionObject as Exception;
    MessageBox.Show(ex.Message, "error");
}

```

