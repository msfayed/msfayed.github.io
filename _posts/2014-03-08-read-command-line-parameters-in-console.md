---
layout: default
title: Read Command Line Parameters In Console Application
date: '2014-03-08T12:26:00.000-04:00'
author: Mohammed Fayed
tags:
- C#
modified_time: '2017-07-16T02:59:32.916-03:00'
blogger_id: tag:blogger.com,1999:blog-3008009747231704115.post-7250267156978213930
blogger_orig_url: https://www.fayed.org/2014/03/read-command-line-parameters-in-console.html
---


whenever you need to make a small application to test some library or a Peace of code or your application does not even need to have a GUI , you may need to deal with a parameters sent to your application .

Command line parameters are sent to the application as an array of `string` ( `string[]` ) and there is 2 ways that you can use to access this array to get parameters values , assuming that your exe file is called **cApp** and is called from command line like :

```shell
> App a1 a2 a3
```

The first way to read the array is using `for` statement

```csharp
using System;

public class cApp
{
    public static void Main(string[] args)
    {
        // Display the number of parameters sent to the application 
        Console.WriteLine("Parameters Count = {0}", args.Length);

        // Display Parameters values
        for(int i = 0; i &lt; args.Length; i++)
        {
            Console.WriteLine("args[{0}] = [{1}]", i, args[i]);
        }
    }
}
```
The result will be like :

```shell
>Parameters Count = 3
>args[0] = [a1]
>args[1] = [a2]
>args[2] = [a3]
```

The second way to read the array using `foreach` statement like the following :


```csharp
using System;

public class cApp
{
   public static void Main(string[] args)
   {
        // Display the number of parameters sent to the application 
        Console.WriteLine("Parameters Count = {0}", args.Length);
      
        // Display Parameters values
        foreach(string str in args)
        {
            Console.WriteLine(str);
        }
   }
}
 
```

The result will be like :

```shell
>Parameters Count = 3
>a1
>a2
>a3
```
