---
layout: default
title: Installing and configuring Opinio Survey
date: '2012-05-20T21:53:00.000-03:00'
author: Mohammed Fayed
tags:
- opinio-survey
- tomcat
modified_time: '2017-07-16T03:32:58.368-03:00'
thumbnail: https://2.bp.blogspot.com/-_tAXoRt9ghk/VHq_rQC99xI/AAAAAAAAieU/qCC0xUoWJWA/s72-c/p1.jpg
blogger_id: tag:blogger.com,1999:blog-3008009747231704115.post-2151188048704723021
blogger_orig_url: https://www.fayed.org/2012/05/installing-and-configuring-opinio-survey.html
---

**So What is Opinio ?**  
  
[Opinio](http://www.objectplanet.com/opinio/) is one of the best surveys systems , it supports multiple users, multilingual surveys, survey collaboration, powerful features such as skip logic and piping, and a highly scalable architecture so it will cover the survey needs for everyone in your organization.  
  

I found my self enforced to install and use it as my company already buy license for it , Opinio is a web application written in Java so it needs a webserver that supports Java ( I used [Tomcat](http://tomcat.apache.org/) ) .

after download Opinio File ( .zip ) and Tomcat installation file ( be aware to download the .exe file not .zip file for easy setup ) also you must download Java SE ( JRE )  
  

**1**\- Frist thing I installed JRE , very easy steps Next .. Next .. Finish ( I Like this type of installations ).  
**2**\- Then I add new environment variable ( JAVA\_HOME ) , you make this by right click on computer under start menu then choose Properties  
  

[![](https://2.bp.blogspot.com/-_tAXoRt9ghk/VHq_rQC99xI/AAAAAAAAieU/qCC0xUoWJWA/s640/p1.jpg)](http://2.bp.blogspot.com/-_tAXoRt9ghk/VHq_rQC99xI/AAAAAAAAieU/qCC0xUoWJWA/s1600/p1.jpg)

  
from the new window choose Advanced system settings –> Environment Variables , then from the opened window click on New in the bottom of the window , enter JAVA\_HOME as variable name and the Path to Java installation folder in the value.

  
  

[![](https://4.bp.blogspot.com/--yYL5JPjEqI/VHq_rT03zfI/AAAAAAAAiec/1cSu2gWq2oc/s640/p2.jpg)](http://4.bp.blogspot.com/--yYL5JPjEqI/VHq_rT03zfI/AAAAAAAAiec/1cSu2gWq2oc/s1600/p2.jpg)

  
  
**3**\- then I installed Tomcat it was also very easy just you need to enter user name and password for Tomcat Administrator and the Port that the webserver (Tomcat) will listen to . ( in my case I make this port 8080 as my server already contain IIS which listen to Port 80 ).  
  

[![](https://1.bp.blogspot.com/-YNVtEZfNLeo/VHq_vYiUgtI/AAAAAAAAie0/nsfoScZUW8E/s640/p3.jpg)](http://1.bp.blogspot.com/-YNVtEZfNLeo/VHq_vYiUgtI/AAAAAAAAie0/nsfoScZUW8E/s1600/p3.jpg)

  
**4**\- to test that Tomcat is working fine I opened the browser and typed ( http://localhost:8080 ) I found tomcat default page which is good .

  
**5**\- Then I extracted opinio.war from Opinio zip file to \[Tomcat Folder\]\\webapps then I restarted the Tomcat Service ( From Start menu there is shortcut for managing Tomcat ) and it automatically create opinio folder and configure it

  
**6**\- by testing local URL ( http://localhost:8080/opinio ) I found Opinio is working just fine. but when I accessed this URL from my PC over the internet ( by replacing <localhost> with the server IP ) it didn’t work , finally I figure it out it’s the firewall ![](https://2.bp.blogspot.com/-u3UwbWbm6i8/VHq_r-6RWZI/AAAAAAAAieg/8DAc32_Kpw8/s1600/smiley-wink.gif) I just opened the 8080 port and it works .