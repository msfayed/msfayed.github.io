---
layout: default
title: Remove Extra Spaces From A String
date: '2014-01-05T08:27:00.000-04:00'
author: Mohammed Fayed
tags:
- C#
modified_time: '2017-07-16T03:14:02.108-03:00'
blogger_id: tag:blogger.com,1999:blog-3008009747231704115.post-3875534642015763783
blogger_orig_url: https://www.fayed.org/2014/01/remove-extra-spaces-from-string.html
---


Use the following extension method to remove the extra spaces ( in the middle ) from a desired string , its easy right :)

```csharp
public static string RemoveWhiteSpace(this string mStr)
{
    string mRet = mStr.Trim().Replace("  ", " ");
    while (mRet.Contains("  "))
      mRet = mRet.Replace("  ", " ");
 
    return mRet.Trim();
}

```
