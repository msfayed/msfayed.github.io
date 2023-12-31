---
layout: default
title: Resize WinForms Button Image
date: '2018-10-24T07:53:00.000-03:00'
author: Mohammed Fayed
tags:
- Button
- WinForms
- C#
- GIF
modified_time: '2018-10-24T07:53:02.411-03:00'
blogger_id: tag:blogger.com,1999:blog-3008009747231704115.post-7507119782299522453
blogger_orig_url: https://www.fayed.org/2018/10/resize-winforms-button-image.html
---



Resize WinForms Button Image ( support time Animated GIF )

```csharp
using System;
using System.Collections.Generic;
using System.Drawing;
using System.Drawing.Imaging;
using System.Windows.Forms;

public static class ButtonEx
{
    public static void ResizeImage(this Button btn, Size mSize)
    {
        if (btn.Image == null)
            return;

        var mImageFrames = AnimatedGif.ExtractFrames(btn.Image, mSize);

        if (mImageFrames.Count > 0)
        {
            btn.Image = mImageFrames[0].Image;

            if (mImageFrames.Count > 1)
            {
                var mIndex = 0;
                var mTimer = new Timer();

                mTimer.Interval = mImageFrames[0].Duration * 15;


                mTimer.Tick += (object sender, EventArgs e) =>
                {
                    mTimer.Stop();

                    if (mIndex < mImageFrames.Count - 1)
                        mIndex++;
                    else
                        mIndex = 0;

                    btn.Image = mImageFrames[mIndex].Image;
                    mTimer.Interval = mImageFrames[mIndex].Duration * 15;
                    mTimer.Start();
                };

                mTimer.Start();
            }
        }
    }

    public static Image Resize(this Image mImg, Size mSize)
    {
        Bitmap newImage = new Bitmap(mSize.Width, mSize.Height);
        using (Graphics gr = Graphics.FromImage(newImage))
        {
            gr.SmoothingMode = System.Drawing.Drawing2D.SmoothingMode.HighQuality;
            gr.InterpolationMode = System.Drawing.Drawing2D.InterpolationMode.HighQualityBicubic;
            gr.PixelOffsetMode = System.Drawing.Drawing2D.PixelOffsetMode.HighQuality;
            gr.DrawImage(mImg, new Rectangle(0, 0, mSize.Width, mSize.Height));
        }
        return newImage;
    }

    public class AnimatedGif
    {
        public static List<animatedgifframe> ExtractFrames(Image img, Size? mNewSize = null)
        {
            var mImages = new List<animatedgifframe>();

            try
            {
                int frames = img.GetFrameCount(FrameDimension.Time);
                if (frames <= 1) throw new ArgumentException("Image not animated");
                byte[] times = img.GetPropertyItem(0x5100).Value;
                int frame = 0;
                for (; ; )
                {
                    int dur = BitConverter.ToInt32(times, 4 * frame);

                    if (mNewSize == null)
                        mImages.Add(new AnimatedGifFrame(new Bitmap(img), dur));
                    else
                        mImages.Add(new AnimatedGifFrame(new Bitmap(img).Resize(mNewSize.Value), dur));

                    if (++frame >= frames)
                        break;

                    img.SelectActiveFrame(FrameDimension.Time, frame);
                }
                img.Dispose();
            }
            catch
            {
                if (mImages.Count == 0)
                {
                    if (mNewSize == null)
                        mImages.Add(new AnimatedGifFrame(img, 1000));
                    else
                        mImages.Add(new AnimatedGifFrame(img.Resize(mNewSize.Value), 1000));
                }
            }
            
            return mImages;
        }

        public class AnimatedGifFrame
        {
            private int mDuration;
            private Image mImage;
            internal AnimatedGifFrame(Image img, int duration)
            {
                mImage = img; mDuration = duration;
            }
            public Image Image { get { return mImage; } }
            public int Duration { get { return mDuration; } }
        }
    }
}

```


#### source: 

- [https://social.microsoft.com/Forums/en-US/fcb7d14d-d15b-4336-971c-94a80e34b85e/editing-animated-gifs-in-c?forum=netfxbcl](https://www.blogger.com/goog_1696570594) 
- [https://stackoverflow.com/a/87786/1312036](https://stackoverflow.com/a/87786/1312036)
