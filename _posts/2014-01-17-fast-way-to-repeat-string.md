---
layout: default
title: Fast Way To Repeat a String
date: '2014-01-17T10:46:00.000-04:00'
author: Mohammed Fayed
tags:
- C#
modified_time: '2017-07-16T03:12:48.185-03:00'
blogger_id: tag:blogger.com,1999:blog-3008009747231704115.post-8179870251624709947
blogger_orig_url: https://www.fayed.org/2014/01/fast-way-to-repeat-string.html
---


there is may way to do it , but I this the following extension method will be that fast and efficient way to repeat a string for a specific number of times , just add this method to a static class and you can call it like :

```csharp
" ".Repeat(10);

public static string Repeat(this string value, int count)
{
    return new StringBuilder().Insert(0, value, count).ToString();
}
```