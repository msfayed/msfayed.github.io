---
layout: default
title: Convert Image to Byte Array
date: '2013-05-09T19:30:00.000-03:00'
author: Mohammed Fayed
tags:
- C#
modified_time: '2017-07-16T03:17:38.248-03:00'
blogger_id: tag:blogger.com,1999:blog-3008009747231704115.post-8065567222873745236
blogger_orig_url: https://www.fayed.org/2013/05/convert-image-to-byte-array.html
---

As the subject says ... if you want to convert an Image to an array of bytes to save it in the database or in a file or send it to a web service or another application , just put the following extension method in a static class and call it from the Image object like :  
  

```cs
byte[] mImageBytes = img.ToBytes(ImageFormat.Png);

public static byte[] ToBytes(this Image image, ImageFormat format)
{
    if (image == null)
       throw new ArgumentNullException("image");
    if (format == null)
       throw new ArgumentNullException("format");
 
    using (MemoryStream stream = new MemoryStream())
    {
       image.Save(stream, format);
       return stream.ToArray();
    }
}
```