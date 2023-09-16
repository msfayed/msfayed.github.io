---
layout: default
title: Convert Text To Code ( Put it in a string variable )
date: '2014-03-09T09:02:00.000-03:00'
author: Mohammed Fayed
tags:
- C#
modified_time: '2017-07-16T02:46:21.039-03:00'
thumbnail: https://1.bp.blogspot.com/-DGQRGGT2dTE/VzPRhyTA1pI/AAAAAAAAAFY/bYR9Jxc47w0VcIIQqgWtXh1IjgSaSTingCK4B/s72-c/img1.png
blogger_id: tag:blogger.com,1999:blog-3008009747231704115.post-8778640840376297289
blogger_orig_url: https://www.fayed.org/2014/03/convert-text-to-code-put-it-in-string.html
---

covert a text block to code ( text stored in a variable ) with an option to produce C# or VB.NET code using `String` or  `StringBuilder`

**Usage :**  When you want to display a large text message or send an email with specific template you will need to format your text in a text editor like notepad or what ever you use but when you start to copy this text to your code editor to deal with you will need this utility

**The Code :**

create as GUI like the bellow

[![](https://1.bp.blogspot.com/-DGQRGGT2dTE/VzPRhyTA1pI/AAAAAAAAAFY/bYR9Jxc47w0VcIIQqgWtXh1IjgSaSTingCK4B/s640/img1.png)](http://1.bp.blogspot.com/-DGQRGGT2dTE/VzPRhyTA1pI/AAAAAAAAAFY/bYR9Jxc47w0VcIIQqgWtXh1IjgSaSTingCK4B/s1600/img1.png)

Then add the following code to the input text ( TextChanged ) Event

```csharp
if (this.cmbLanguage.SelectedIndex == 1)
{
 txtTextToCodeResult.Text= this.convertToVBCode(txtTextToCodeInput.Lines, this.chkUseStringBuilder.Checked,this.chkAddLineBreak.Checked);
}
else
{
 txtTextToCodeResult.Text= this.convertToCSharpCode(txtTextToCodeInput.Lines, this.chkUseStringBuilder.Checked,this.chkAddLineBreak.Checked);
}
```

Then add the following methods to your form :

```csharp
private string convertToCSharpCode(string[] lines, bool useStringBuilder, bool addNewLineSymbole)
{
    StringBuilder output = new StringBuilder(0x400);
    if (useStringBuilder)
    {
       output.Append("StringBuilder str = new StringBuilder(1024);\r\n\r\n");
       foreach (string l in lines)
       {
          output.Append(string.Format("str.Append (@\"{0}\"{1}); {2}", l.Replace("\"", "\"\""), addNewLineSymbole ? " + Environment.NewLine " : "", "\r\n"));
       }
    }
    else
    {
       output.Append("String str = \"\";\r\n\r\n");
       foreach (string l in lines)
       {
           output.Append(string.Format("str += @\"{0}\"{1}; {2}", l.Replace("\"", "\"\""), addNewLineSymbole ? " + Environment.NewLine " : "", "\r\n"));
       }
    }
    return output.ToString();
}

private string convertToVBCode(string[] lines, bool useStringBuilder, bool addNewLineSymbole)
{
    StringBuilder output = new StringBuilder(0x400);
    if (useStringBuilder)
    {
      output.Append("Dim str As New System.Text.StringBuilder(1024)\r\n\r\n");
      foreach (string l in lines)
      {
         output.Append(string.Format("str.Append (\"{0}\"{1}) {2}", l.Replace("\"", "\"\""), addNewLineSymbole ? " &amp; Environment.NewLine " : "", "\r\n"));
       }
    }
    else
    {
       output.Append("Dim str As String = \"\"\r\n\r\n");
       foreach (string l in lines)
       {
           output.Append(string.Format("str &amp;= \"{0}\"{1} {2}", l.Replace("\"", "\"\""), addNewLineSymbole ? " &amp; Environment.NewLine " : "", "\r\n"));
       }
    }
    return output.ToString();
}

```
