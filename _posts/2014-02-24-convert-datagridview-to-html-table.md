---
layout: default
title: Convert DataGridView to HTML Table
date: '2014-02-24T08:27:00.000-04:00'
author: Mohammed Fayed
tags:
- C#
modified_time: '2017-07-16T03:09:02.551-03:00'
blogger_id: tag:blogger.com,1999:blog-3008009747231704115.post-3252838532469110447
blogger_orig_url: https://www.fayed.org/2014/02/convert-datagridview-to-html-table.html
---


Whenever you want to convert the data in `DataGridView` you can use the following method , the process is very easy , put this method in any common class in your application an call it , it will return a string contain the HTML Table.


```csharp

public static string ConvertToHtmlTable(DataGridView targetTable)
{
    string myHtmlFile = "";

    if (targetTable == null)
    {
        throw new System.ArgumentNullException("targetTable");
    }

    var myBuilder = new StringBuilder();

    myBuilder.Append("&lt;html");
    myBuilder.Append("&lt;head&gt;");
    myBuilder.Append("&lt;title&gt;");
    myBuilder.Append("Page-");
    myBuilder.Append(Guid.NewGuid().ToString());
    myBuilder.Append("&lt;/title&gt;");
    myBuilder.Append("&lt;/head&gt;");
    myBuilder.Append("&lt;body&gt;");
    myBuilder.Append("&lt;table border='1px' cellpadding='5' cellspacing='0' ");
    myBuilder.Append("style='border: solid 1px Silver;'&gt;");

    myBuilder.Append("&lt;tr align='left' valign='top'&gt;");

    foreach (DataGridViewColumn myColumn in targetTable.Columns)
    {
        if (CanExport(myColumn.HeaderText))
        {
            myBuilder.Append("&lt;td align='left' valign='top'&gt;");
            myBuilder.Append(myColumn.HeaderText);
            myBuilder.Append("&lt;/td&gt;");
        }
    }

    myBuilder.Append("&lt;/tr&gt;");

    //Add the data rows.
    foreach (DataGridViewRow myRow in targetTable.Rows)
    {
        myBuilder.Append("&lt;tr align='left' valign='top'&gt;");

        foreach (DataGridViewColumn myColumn in targetTable.Columns)
        {
            if (CanExport(myColumn.HeaderText))
            {
                myBuilder.Append("&lt;td align='left' valign='top'&gt;");
                myBuilder.Append(myRow.Cells[myColumn.HeaderText].Value.ToString());
                myBuilder.Append("&lt;/td&gt;");
            }
        }

        myBuilder.Append("&lt;/tr&gt;");
    }

    //Close tags.
    myBuilder.Append("&lt;/table&gt;");
    myBuilder.Append("&lt;/body&gt;");
    myBuilder.Append("&lt;/html&gt;");

    //Get the string for return.
    myHtmlFile = myBuilder.ToString();

    return myHtmlFile;
}

```
