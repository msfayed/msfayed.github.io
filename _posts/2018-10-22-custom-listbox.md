---
layout: default
title: Custom ListBox
date: '2018-10-22T03:26:00.001-03:00'
author: Mohammed Fayed
tags:
- ListBox
- C#
- UserControl
modified_time: '2018-10-22T03:26:50.091-03:00'
blogger_id: tag:blogger.com,1999:blog-3008009747231704115.post-3230558839470808333
blogger_orig_url: https://www.fayed.org/2018/10/custom-listbox.html
---

Just a custom ListBox to hold any Control, made in C# to hold my UserControls :


```csharp
 
using System.Windows.Forms;

public class GList : FlowLayoutPanel
{
    public GList()
    {
        this.AutoScroll = true;
        this.AutoSize = false;
        this.FlowDirection = FlowDirection.TopDown;
        this.Margin = this.Padding = new Padding(0, 0, 0, 0);
        this.WrapContents = false;
        this.BorderStyle = BorderStyle.FixedSingle;
    }

    public void Add(Control mCtrl)
    {
        base.Controls.Add(mCtrl);
    }

    public void Remove(string Name)
    {
        var c = base.Controls[Name];
        base.Controls.Remove(c);
        c.Dispose();
    }

    public void RemoveAt(int Index)
    {
        var c = base.Controls[Index];
        base.Controls.Remove(c);
        c.Dispose();
    }

    public void Clear()
    {
        while (base.Controls.Count > 0)
        {
            var c = base.Controls[0];
            base.Controls.Remove(c);
            c.Dispose();
        }
    }

    protected override void OnPaint(PaintEventArgs e)
    {
        SetupAnchors();
        base.OnPaint(e);
    }

    void SetupAnchors()
    {
        if (base.Controls.Count > 0)
        {
            for (int i = 0; i < base.Controls.Count; i++)
            {
                var c = base.Controls[i];
                if (i == 0)
                {
                    c.Anchor = AnchorStyles.Left | AnchorStyles.Top;
                    c.Width = this.Width - SystemInformation.VerticalScrollBarWidth - 10;
                }
                else
                    c.Anchor = AnchorStyles.Left | AnchorStyles.Right;
            }
        }
    }
}


```


#### source: 
- [https://www.codeproject.com/Articles/333864/Flexible-List-Control](https://www.codeproject.com/Articles/333864/Flexible-List-Control)


