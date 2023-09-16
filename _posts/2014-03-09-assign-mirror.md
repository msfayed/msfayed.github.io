---
layout: default
title: Assign Mirror
date: '2014-03-09T20:10:00.000-03:00'
author: Mohammed Fayed
tags:
- C#
modified_time: '2017-07-12T08:56:23.290-03:00'
thumbnail: https://3.bp.blogspot.com/-jXh4S1gvex4/VzPP51ouPtI/AAAAAAAAAFA/_cQk6jhr_QkFD6caej0K3BWVIeGx_tDRQCK4B/s72-c/img1.png
blogger_id: tag:blogger.com,1999:blog-3008009747231704115.post-89822552563683254
blogger_orig_url: https://www.fayed.org/2014/03/assign-mirror.html
---


Assign Mirror is a small tool that is mirror the assign statement ( replace the left side with the right side and the right side with left side ) 

**Usage :**  assume you are writing a code in your database application to read the values from your business object and puts the values in a controls on your form ( about 30 field ) and now you want to save the values form the form controls to your object so you can save it in the database again , the fast method will be by copying your previous code and change the  left side of the assign operator with the right side .

but if you have a tool that make this for you, you are going to save time and type mistakes :)

**The Code :**

create as GUI like the bellow screen shoot

[![](https://3.bp.blogspot.com/-jXh4S1gvex4/VzPP51ouPtI/AAAAAAAAAFA/_cQk6jhr_QkFD6caej0K3BWVIeGx_tDRQCK4B/s640/img1.png)](http://3.bp.blogspot.com/-jXh4S1gvex4/VzPP51ouPtI/AAAAAAAAAFA/_cQk6jhr_QkFD6caej0K3BWVIeGx_tDRQCK4B/s1600/img1.png)

Then add the following code to the ( Mirror ) Button

```csharp
bool mAddSemicolon = false;
foreach (string mStr in txtMirrorSource.Lines)
{
    if (mStr.Contains("="))
    {
        string mTempStr = "";

        if (mStr.Contains("//"))
           mTempStr = mStr.Substring(0, mStr.IndexOf("//")).Trim();
        else if (mStr.Contains(";"))
           mTempStr = mStr.Substring(0, mStr.IndexOf(";") + 1);
        else
           mTempStr = mStr.Trim();

        mAddSemicolon = mTempStr.EndsWith(";");

        txtMirrorDist.AppendText(mTempStr.Split('=')[1].Replace(";", "").Trim());
        txtMirrorDist.AppendText(" = ");
        txtMirrorDist.AppendText(mTempStr.Split('=')[0].Trim());

        if (mAddSemicolon) txtMirrorDist.AppendText(" ;");

        txtMirrorDist.AppendText(Environment.NewLine);
    }
 }

```