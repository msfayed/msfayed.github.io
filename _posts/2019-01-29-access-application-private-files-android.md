---
layout: default
title: Access Application Private Files in Android
date: '2019-01-29T07:01:00.001-04:00'
author: Mohammed Fayed
tags:
- Backup
- ADB
- Android
modified_time: '2019-01-29T07:13:57.285-04:00'
blogger_id: tag:blogger.com,1999:blog-3008009747231704115.post-2986523341472736996
blogger_orig_url: https://www.fayed.org/2019/01/access-application-private-files-android.html
---

I came across an application that I want to explore it's private files (especially the downloaded media files) and after searching some try and error I found that an easy way to to it is by using ADB tools to backup the application to my PC and then decompress and decrypt  the backup file (because my mobile is encrypted and so the backup must be encrypted too) and then extract the media files :)

1- I used [this ADB installer](https://forum.xda-developers.com/showthread.php?t=2588979) so I don't have to install the full SDK.

2- Then I downloaded  [android-backup-extractor](https://github.com/nelenkov/android-backup-extractor/releases) to extract the the backup file.
3- if you are using Java 8 Update 161 or later skip this step as  [Oracle's release notes for Java 8 Update 161](http://www.oracle.com/technetwork/java/javase/8u161-relnotes-4021379.html#JDK-8170157), stated that unlimited cryptography is enabled by default.

and to be able to decrypt the backup file you will need to download [Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction Policy Files 7](https://www.oracle.com/technetwork/java/javase/downloads/jce-7-download-432124.html) or  [Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction Policy Files 8](https://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html) according to your java run-time version.

then I executed the bellow command :

```shell
adb backup -noapk com.MyApp.AppName
```

then

```shell
java -jar abe-all.jar unpack backup.ab backup.ab.tar [MyPassword]
```

like :

```shell
C:\adb
λ adb backup -noapk com.MyApp.AppName
Now unlock your device and confirm the backup operation.

C:\adb
λ java -jar abe-all.jar unpack backup.ab backup.ab.tar [MyPassword]
Calculated MK checksum (use UTF-8: true): C15A850070F1791D786135F7ECCC96053B63F55374EB46CEFA4C52C1B3219D44
0% 1% 2% 3% 4% 5% 6% 7% 8% 9% 10% 11% 12% 13% 14% 15% 16% 17% 18% 19% 20% 21% 22% 23% 24% 25% 26% 27% 28% 29% 30% 31% 32% 33% 34% 35% 36% 37% 38% 39% 40% 41% 42% 43% 44% 45% 46% 47% 48% 49% 50% 51% 52% 53% 54% 55% 56% 57% 58% 59% 60% 61% 62% 63% 64% 65% 66% 67% 68% 69% 70% 71% 72% 73% 74% 75% 76% 77% 78% 79% 80% 81% 82% 83% 84% 85% 86% 87% 88% 89% 90% 91% 92% 93% 94% 95% 96% 97% 98% 99% 100%
311480832 bytes written to backup.ab.tar.

```

