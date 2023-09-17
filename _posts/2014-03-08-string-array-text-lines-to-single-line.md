---
layout: default
title: String Array (Text Lines ) To Single Line
date: '2014-03-08T20:13:00.000-04:00'
author: Mohammed Fayed
tags:
- C#
modified_time: '2017-07-16T02:49:43.410-03:00'
thumbnail: https://3.bp.blogspot.com/-L6izKz53L9s/VzPTFmM3N0I/AAAAAAAAAFk/2WBi_snjaF4VbgEmkACk8QvG9Ngphe3WQCK4B/s72-c/img1.png
blogger_id: tag:blogger.com,1999:blog-3008009747231704115.post-1914694323900550937
blogger_orig_url: https://www.fayed.org/2014/03/string-array-text-lines-to-single-line.html
---

<div dir="ltr" style="text-align: left;" trbidi="on">
Array To Line is a small tool that is meant to convert an array of lines to single line of text with proper formatting and concatenation string.

**Usage :**  assume you are executing a SQL query against your database and you are using then IN operator that allows you to specify multiple values in a WHERE clause , but the values here is from an EXCEL Sheet ( column ) with about 100 or more value !! of course you will hate your self while your are formatting the SQL statement and you will hate your self more if you made a mistake .

but if have only to copy all values to a Text Box in your tool and click a Button on the screen and get the result values with the proper quotes and separator ready for execute I think that you will be happy :)

**The Code :**
create as GUI like the bellow screen shoot


[![](https://3.bp.blogspot.com/-L6izKz53L9s/VzPTFmM3N0I/AAAAAAAAAFk/2WBi_snjaF4VbgEmkACk8QvG9Ngphe3WQCK4B/s640/img1.png)](http://3.bp.blogspot.com/-L6izKz53L9s/VzPTFmM3N0I/AAAAAAAAAFk/2WBi_snjaF4VbgEmkACk8QvG9Ngphe3WQCK4B/s1600/img1.png)


Name the TextBoxes like:

```
Input TextBox --> txtArrayInput
Separator TextBox --> txtSeparator
Quotes TextBox --> txtQuotes
Result TextBox --> txtArrayResult
```

And the CheckBoxes like : 

```
Ignore Empty Line --> chkIgnoreEmptyLine
Trim ( to apply on each line ) --> chkTrim
```

put the following code in the ( Combine ) Button Click Event 

```csharp
 var myStr = new StringBuilder();
 int mCount = 0;
 for (int ii = 0; ii < txtArrayInput.Lines.Length; ii++)
 {
     if (chkIgnoreEmptyLine.Checked)
     {
         if (txtArrayInput.Lines[ii].Trim().Length > 0)
         {

            myStr.Append(txtQuotes.Text + ((chkTrim.Checked) ? txtArrayInput.Lines[ii].Trim() : txtArrayInput.Lines[ii]) + txtQuotes.Text + txtSeparator.Text);
            mCount++;
   
         }
     }
     else
     {
        myStr.Append(txtQuotes.Text + ((chkTrim.Checked) ? txtArrayInput.Lines[ii].Trim() : txtArrayInput.Lines[ii]) +     txtQuotes.Text + txtSeparator.Text);
        mCount++;
        
     }
 }

 txtArrayResult.Text = myStr.ToString(); 
 lblItemsCount.Text = mCount.ToString() + " Items Combined ";

```