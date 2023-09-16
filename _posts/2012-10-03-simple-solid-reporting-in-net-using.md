---
layout: default
title: Simple & Solid Reporting in .NET using ABCpdf.NET
date: '2012-10-03T23:22:00.000-03:00'
author: Mohammed Fayed
tags:
- C#
- ABCpdf.NET
- PDF
modified_time: '2017-07-16T03:26:58.145-03:00'
thumbnail: https://1.bp.blogspot.com/-oQtfgp1SS_M/VzOxRRGtnlI/AAAAAAAAAEI/CbAUJnzNvqQRzzwBdS8whzYFaUdbMhBIgCLcB/s72-c/img1.png
blogger_id: tag:blogger.com,1999:blog-3008009747231704115.post-2284212767067580876
blogger_orig_url: https://www.fayed.org/2012/10/simple-solid-reporting-in-net-using.html
---

last week I was involved in developing an internal windows application for my company and we wanted to print simple report and keep the application simple to deploy ( xcopy way ) and as we use Windows 7 in all our company computers which already have .NET framework but nothing else ( not crystal reports or MS reporting tools ) , I decided to use [ABCpdf.NET](http://www.websupergoo.com/) to create our simple report via creating html page and converting it to PDF for printing & archiving .  
  
hint : if you don’t have [ABCpdf.NET](http://www.websupergoo.com/) already installed you can download it from this link [http://www.websupergoo.com/download.htm](http://www.websupergoo.com/download.htm) ( the 32bit version is about 43MB )  
  
here I am going to explain the first steps to get started with [ABCpdf.NET](http://www.websupergoo.com/) library ..  
  
\* after downloading and installing [ABCpdf.NET](http://www.websupergoo.com/) library go to Visual Studio and create new windows forms application  
  
\* add webBrowser control to show the report and a button to save it in PDF file format like this image  
  
  

[![](https://1.bp.blogspot.com/-oQtfgp1SS_M/VzOxRRGtnlI/AAAAAAAAAEI/CbAUJnzNvqQRzzwBdS8whzYFaUdbMhBIgCLcB/s1600/img1.png)](https://1.bp.blogspot.com/-oQtfgp1SS_M/VzOxRRGtnlI/AAAAAAAAAEI/CbAUJnzNvqQRzzwBdS8whzYFaUdbMhBIgCLcB/s1600/img1.png)

  
\* add HTML page to your project and put the following HTML in it :  
  

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN"> 
<html> 
<head> 
    <title></title> 
</head> 
<body> 
    <p> 
        Fayecom Company 
        <br /> 
        Simple Report 
        Test</p> 
    <br /> 
    <br /> 
    <table width="100%"> 
        <tr style="font-weight: bold; background-color: #E0E0E0"> 
            <td> 
                Client Name 
            </td> 
            <td> 
                Client Mobile Phone 
            </td> 
            <td> 
                Client Address 
            </td> 
        </tr> 
        <tr> 
            <td> 
                \[name\] 
            </td> 
            <td> 
                \[mobile\] 
            </td> 
            <td> 
                \[address\] 
            </td> 
        </tr> 
    </table> 
</body> 
</html>

  
\* change \[copy to output Directory\] property of this HTML file to ( copy if newer ) or ( always copy )  

  
\* write the following code to form load event  
  

string mFileName = Path.Combine(Path.GetDirectoryName(Application.ExecutablePath) ,"HTMLPage1.htm");
string mHTML = File.ReadAllText(mFileName); 

mHTML = mHTML.Replace("\[name\]","Mohammed Samir Fayed"); 
mHTML = mHTML.Replace("\[mobile\]","1234567890"); 
mHTML = mHTML.Replace("\[address\]","My Address :)");

webBrowser1.DocumentText = mHTML;

  
\* now run your application , it should be like this  
  
  

[![](https://1.bp.blogspot.com/-5zwQoDSji4M/VzOxcVGav1I/AAAAAAAAAEQ/Q-getYHRsGc-i3z0z2VfGL1TQfR3PJueACKgB/s1600/img2.png)](https://1.bp.blogspot.com/-5zwQoDSji4M/VzOxcVGav1I/AAAAAAAAAEQ/Q-getYHRsGc-i3z0z2VfGL1TQfR3PJueACKgB/s1600/img2.png)

  
  
\* now add reference to [ABCpdf.NET](http://www.websupergoo.com/) library  
  
  

[![](https://1.bp.blogspot.com/-j3kGQTlM55Q/VzOxoRZKtbI/AAAAAAAAAEY/8m8836rDeqA3o3R2fkaMMCuMs6kTEDtdACK4B/s640/img3.png)](http://1.bp.blogspot.com/-j3kGQTlM55Q/VzOxoRZKtbI/AAAAAAAAAEY/8m8836rDeqA3o3R2fkaMMCuMs6kTEDtdACK4B/s1600/img3.png)

  
  
  
\* add this code to button click event  
  

string mPdfFileName = Path.Combine(Path.GetDirectoryName(Application.ExecutablePath) ,"tempFile.pdf");

Doc myPDF = new Doc();

myPDF.AddImageHtml(webBrowser1.DocumentText);

myPDF.Save(mPdfFileName);

System.Diagnostics.Process.Start(mPdfFileName);

  
this will export your report to PDF and run it the default PDF viewer like this  
  
  

[![](https://2.bp.blogspot.com/-oKw4zr_r1xc/VzOx1O80WSI/AAAAAAAAAEk/4gw3H1G8HWgbMsO2lBPCryZr1OGz-m9tQCK4B/s640/img4.png)](http://2.bp.blogspot.com/-oKw4zr_r1xc/VzOx1O80WSI/AAAAAAAAAEk/4gw3H1G8HWgbMsO2lBPCryZr1OGz-m9tQCK4B/s1600/img4.png)