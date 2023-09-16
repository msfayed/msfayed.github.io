---
layout: default
title: Reset Linux [ root ] password
date: '2017-07-27T07:37:00.002-03:00'
author: Mohammed Fayed
tags:
modified_time: '2017-07-27T07:37:36.707-03:00'
blogger_id: tag:blogger.com,1999:blog-3008009747231704115.post-1764177048584226215
blogger_orig_url: https://www.fayed.org/2017/07/reset-linux-root-password.html
---

<div dir="ltr" style="text-align: left;" trbidi="on">
from time to time I forget the password for [root] user in some of my Linux installations , and to reset the password I just follow these steps :

- Press F2 when the splash screen comes up
- A GRUB screen will display
- enter the letter `e` (without quotes)
- Using the arrow keys, move the cursor to the line for kernel
- Enter the letter `e` again.
- You will see a command line
- After the last word/character append a space and the word single (single mode)
- Hit Enter
- Make sure the cursor is on the kernel line
- enter the letter `b` (this will boot)
- System will load into single mode, should see
- Enter command  `$ passwd root`
- Enter new password two times
- Enter exit (this will reboot)
- Test the new root password voila 🙂


source :

- [https://www.blogger.com/goog_1326802336](https://www.blogger.com/goog_1326802336)
- [http://www.oraworld.co.uk/oracle-enterprise-linux-forgot-reset-root-password/](http://www.oraworld.co.uk/oracle-enterprise-linux-forgot-reset-root-password/)

