---
layout: default
title: Re-write Git History - Change Author 
date: '2023-10-08T10:00:00.000-03:00'
author: Mohammed Fayed
tags:
- GIT
modified_time: '2023-10-08T10:00:00.000-03:00'
---

# Re-write Git History - Change Author  

This will re-write all the branch commits with the new supplied name and email

```shell

git filter-branch -f --env-filter "GIT_AUTHOR_NAME='Mohammed Fayed'; GIT_AUTHOR_EMAIL='m@fayed.org'; GIT_COMMITTER_NAME='Mohammed Fayed'; GIT_COMMITTER_EMAIL='m@fayed.org';" HEAD

```