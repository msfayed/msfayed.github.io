---
layout: default
title: Symbolic link (aka Symlinks)
date: '2019-01-24T01:01:00.001-04:00'
author: Mohammed Fayed
tags:
modified_time: '2019-01-24T01:01:14.542-04:00'
blogger_id: tag:blogger.com,1999:blog-3008009747231704115.post-3056015600597562071
blogger_orig_url: https://www.fayed.org/2019/01/symbolic-link-aka-symlinks.html
---


From Wikipedia :

In computing, a symbolic link is a term for any file that contains a reference to another file or directory in the form of an absolute or relative path and that affects path-name resolution.

**How to Create Symbolic Links in Windows :**

we can use `MKLINK` command in windows to create links to folder or files, for folder I use the syntax  :

```shell
> mklink /J Link Target
```

Sample :


```shell
> mklink /j myFolder D:\fayed\myOriginalFolder
Junction created for myFolder  <<===>> D:\fayed\myOriginalFolder

```

**The full command option is :**


```shell
> mklink
Creates a symbolic link.

MKLINK [[/D] | [/H] | [/J]] Link Target

        /D      Creates a directory symbolic link.  Default is a file
                symbolic link.
        /H      Creates a hard link instead of a symbolic link.
        /J      Creates a Directory Junction.
        Link    specifies the new symbolic link name.
        Target  specifies the path (relative or absolute) that the new link
                refers to.

```


