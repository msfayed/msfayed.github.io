---
layout: default
title: Windows 10 - VPN Error 809
date: '2020-10-14T13:32:00.005-03:00'
author: Mohammed Fayed
tags:
modified_time: '2020-10-14T13:32:46.077-03:00'
blogger_id: tag:blogger.com,1999:blog-3008009747231704115.post-6471477316322743622
blogger_orig_url: https://www.fayed.org/2020/10/windows-10-vpn-error-809.html
---


### Windows Error 809

> Error 809: The network connection between your computer and the VPN server could not be established because the remote server is not responding. This could be because one of the network devices (e.g, firewalls, NAT, routers, etc) between your computer and the remote server is not configured to allow VPN connections. Please contact your Administrator or your service provider to determine which device may be causing the problem.

To fix this error, a [one-time registry change](https://documentation.meraki.com/MX-Z/Client_VPN/Troubleshooting_Client_VPN#Windows_Error_809) is required because the VPN server and/or client is behind NAT (e.g. home router). run the following from an [elevated command prompt](http://www.winhelponline.com/blog/open-elevated-command-prompt-windows/). You must reboot your PC when finished.

*   For Windows Vista, 7, 8.x and 10

    ```shell
    REG ADD HKLM\\SYSTEM\\CurrentControlSet\\Services\\PolicyAgent /v AssumeUDPEncapsulationContextOnSendRule /t REG\_DWORD /d 0x2 /f
    ```
    
*   For Windows XP ONLY
    
    ```
    REG ADD HKLM\\SYSTEM\\CurrentControlSet\\Services\\IPSec /v AssumeUDPEncapsulationContextOnSendRule /t REG\_DWORD /d 0x2 /f
    ```
    

Although uncommon, some Windows systems disable IPsec encryption, causing the connection to fail. To re-enable it, run the following command and reboot your PC.

*   For Windows XP, Vista, 7, 8.x and 10
    ```
    REG ADD HKLM\\SYSTEM\\CurrentControlSet\\Services\\RasMan\\Parameters /v ProhibitIpSec /t REG\_DWORD /d 0x0 /f
    ```
    

SOURCE: 

- https://github.com/hwdsl2/setup-ipsec-vpn/blob/master/docs/clients.md#windows-error-809
