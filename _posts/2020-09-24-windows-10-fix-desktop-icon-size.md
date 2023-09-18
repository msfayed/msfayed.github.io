---
layout: default
title: Windows 10 - fix desktop icon size
date: '2020-09-24T16:14:00.005-03:00'
author: Mohammed Fayed
tags:
- Windows 10
modified_time: '2020-09-24T16:14:49.235-03:00'
thumbnail: https://1.bp.blogspot.com/-LGmkc_8sVDQ/X2zwA22ZAwI/AAAAAAACExg/fb49MYEriykoBd9VTYa2YHjYEvgcZe7bgCNcBGAsYHQ/s72-w624-c-h356/win10-desktop-icon-size.jpg
blogger_id: tag:blogger.com,1999:blog-3008009747231704115.post-7086700461930510764
blogger_orig_url: https://www.fayed.org/2020/09/windows-10-fix-desktop-icon-size.html
---

I faced an issue, that Windows 10 desktop icons width become too large and only one icon was appearing on the screen, I tried every method and option I know in the GUI but it was not fixed, finally the fix was done through the registry key:

`HKEY_CURRENT_USER\Control Panel\Desktop\WindowMetrics`

I updated the `IconSpacing` `IconVerticalSpacing` and set them to `-1125`

![](https://1.bp.blogspot.com/-LGmkc_8sVDQ/X2zwA22ZAwI/AAAAAAACExg/fb49MYEriykoBd9VTYa2YHjYEvgcZe7bgCNcBGAsYHQ/w624-h356/win10-desktop-icon-size.jpg)

then I signed out and signed in again
