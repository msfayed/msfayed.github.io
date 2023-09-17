---
layout: default
title: Find Numbers Gaps
date: '2014-03-09T19:52:00.000-03:00'
author: Mohammed Fayed
tags:
- C#
modified_time: '2017-07-16T02:44:23.304-03:00'
thumbnail: https://1.bp.blogspot.com/-UKMVFnPuWyg/VzPQ8KzDy6I/AAAAAAAAAFM/A_LLGXBt3s8Iooqkib8idKbXnXUIZhlWACK4B/s72-c/img1.png
blogger_id: tag:blogger.com,1999:blog-3008009747231704115.post-2117429983132928919
blogger_orig_url: https://www.fayed.org/2014/03/find-numbers-gaps.html
---


Numbers Gaps is a small utility that is going to give you the missing numbers in a series .

**Usage :** if you are working with a serial numbers in your application and you want to know if one of the numbers in the middle is deleted it can cause a pain to your mind if you try to do it manually .

but if you have a tool that make this for you , it will be very good.

**The Code :**

create as GUI like the bellow screen shoot

[![](https://1.bp.blogspot.com/-UKMVFnPuWyg/VzPQ8KzDy6I/AAAAAAAAAFM/A_LLGXBt3s8Iooqkib8idKbXnXUIZhlWACK4B/s640/img1.png)](http://1.bp.blogspot.com/-UKMVFnPuWyg/VzPQ8KzDy6I/AAAAAAAAAFM/A_LLGXBt3s8Iooqkib8idKbXnXUIZhlWACK4B/s1600/img1.png)


Then add the following code to the ( Find Gaps) Button

```csharp
int[] myData = StringToInts(txtsource.Text);

int mMin = myData.Min();
int mMax = myData.Max();

StringBuilder myStr = new StringBuilder();
int mCount = 0;
for (int ii = mMin; ii <= mMax; ii++)
{
    if (myData.Contains(ii) == false)
    {
        myStr.Append(ii.ToString() + Environment.NewLine);
        mCount++;
    }
}

txtResult.Text = myStr.ToString();
lblCount.Text = "Total Numbers Found : " + mCount.ToString();
```

and add the following method to the same class

```csharp
private int[] StringToInts(string myString)
{
  List ints = new List();
  string[] strings;

  if (myString.Contains(","))
    strings = myString.Split(',');
  else
    strings = myString.Split(Environment.NewLine.ToCharArray());

  foreach (string s in strings)
  {
    int i;
    if (int.TryParse(s.Trim(), out i))
    {
      ints.Add(i);
    }
  }
    return ints.ToArray();
}
```
