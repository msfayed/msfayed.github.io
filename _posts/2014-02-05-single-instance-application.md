---
layout: default
title: Single Instance Application
date: '2014-02-05T03:48:00.000-04:00'
author: Mohammed Fayed
tags:
- C#
modified_time: '2017-07-16T03:10:42.268-03:00'
blogger_id: tag:blogger.com,1999:blog-3008009747231704115.post-7823702529265335297
blogger_orig_url: https://www.fayed.org/2014/02/single-instance-application.html
---


sometimes you want to allow the user to run only one instance of your application , so you will need to check if your application is already running at the start up by using the following method which will return null if there is no previous instance is already running otherwise it will return a `Process` object pointing to that instance   ... enjoy 

```csharp

public static Process PriorProcess()
{
    Process curr = Process.GetCurrentProcess();

    Process[] procs = Process.GetProcessesByName(curr.ProcessName);
    foreach (Process p in procs)
    {
        if ((p.Id != curr.Id) && (p.MainModule.FileName == curr.MainModule.FileName))
        return p;
    }
    
    return null;
}

```
