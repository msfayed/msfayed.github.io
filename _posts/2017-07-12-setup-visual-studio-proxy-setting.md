---
layout: default
title: Setup Visual Studio Proxy Setting
date: '2017-07-12T08:47:00.001-03:00'
author: Mohammed Fayed
tags:
- visual-studio
modified_time: '2017-07-16T02:31:06.784-03:00'
blogger_id: tag:blogger.com,1999:blog-3008009747231704115.post-4811919240804881808
blogger_orig_url: https://www.fayed.org/2017/07/setup-visual-studio-proxy-setting.html
---


If you are working with Visual Studio behind your company proxy you may face many issues like failed to load **nuget** packages ,failed to load Visual Studio **Extensions and Updates** or failed to login with your Microsoft account within Visual Studio.

To solve these problems we have to let visual studio use our proxy correctly , to setup the proxy settings :

Open visual studio folder ,according to your version it may be something like `C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE`

Open ( `devenv.exe.config` ) file and inside `<system.net>` node add the bellow :

```xml
<system.net> 
  <defaultProxy useDefaultCredentials="true" enabled="true"> 
    <proxy bypassonlocal="true" proxyaddress="http://[proxyURL]:[Port]" />
  </defaultProxy>
</system.net>
```

if you got an error like:

```
System.Net.WebException: The underlying connection was closed: An unexpected error occurred on a send.......
```

it mean that you need to force the .NET framework to use **TLS 1.2** when connecting using  HTTPS , to solve this error just create a new **.reg** file and put the following text in it and run it .

```shell
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319]
"SchUseStrongCrypto"=dword:00000001

[HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\.NETFramework\v4.0.30319]
"SchUseStrongCrypto"=dword:00000001
```

but based on you proxy you may face the : 407 error (Proxy authentication required) when **Team Explorer** trying to connect to your **TFS** server , to solve this issue you have to create a DLL with a custom proxy module that provides the credentials and the code will be something like :

```csharp
using System;
using System.Net;
 
namespace Fayed.AuthProxy
{
    public class AuthProxyModule : IWebProxy
    {
        ICredentials crendential = new NetworkCredential("proxy.user", "password");
 
        public ICredentials Credentials
        {
            get
            {
                return crendential;
            }
            set
            {
                crendential = value;
            }
        }
 
        public Uri GetProxy(Uri destination)
        {
            return new Uri("http://proxy:8080", UriKind.Absolute);
        }
 
        public bool IsBypassed(Uri host)
        {
            return host.IsLoopback;
        }
    }
}
```

and the configuration will be like :

```xml
<system.net="">
 <defaultproxy>
    <module type="Fayed.AuthProxy.AuthProxyModule, Fayed.AuthProxy">
    </module>
 </defaultproxy>
</system.net>
```


That's it :)

#### source: 
- [https://msdn.microsoft.com/en-us/library/dn771556(v=vs.120).aspx](https://msdn.microsoft.com/en-us/library/dn771556(v=vs.120).aspx) 
- [https://blogs.msdn.microsoft.com/rido/2010/05/06/how-to-connect-to-tfs-through-authenticated-web-proxy/](https://blogs.msdn.microsoft.com/rido/2010/05/06/how-to-connect-to-tfs-through-authenticated-web-proxy/) 
- [https://stackoverflow.com/a/38053045/1312036](https://stackoverflow.com/a/38053045/1312036)