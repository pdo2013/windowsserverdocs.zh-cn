---
title: "使用 Windows Server Essentials 日志收集"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c6985518-b42d-4cfb-9761-eaa75306b6d7
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d003c6a45159548f7e34d86ca242f74868659d2f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="use-the-windows-server-essentials-log-collector"></a>使用 Windows Server Essentials 日志收集

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

在解决计算机问题时，来自 Microsoft 客户服务和支持的代表可能会要求你从服务器、 计算机在网络，或者同时通过使用 Windows Server Essentials 日志收集收集日志。  
  
 日志收集将计划日志、 事件审阅日志，以及相关的环境的信息复制到一个 zip 文件上的指定位置。 在网络上，或通过远程连接到计算机，您可以直接从该服务器或的任何计算机运行日志收集。  
  
> [!NOTE]
>  -   日志收集不分析网络问题或对任何服务器或网络上的计算机进行更改。 有关如何解决网络问题的信息，请参阅为服务器产品的帮助文档。  
> -   在本指南，你的网络，而不是你的服务器上的计算机称为网络计算机。  
> -   [下载 Windows Server Essentials 日志收集安装程序包](https://go.microsoft.com/fwlink/?LinkID=266341)。  
  
 安装和运行日志收集，请执行以下主题中的步骤：  
  

1.  [安装日志收集](Install-the-Windows-Server-Essentials-Log-Collector.md)  
  
2.  [运行日志收集](Run-the-Windows-Server-Essentials-Log-Collector.md)  

1.  [安装日志收集](../support/Install-the-Windows-Server-Essentials-Log-Collector.md)  
  
2.  [运行日志收集](../support/Run-the-Windows-Server-Essentials-Log-Collector.md)  

  
## <a name="environment-information-collected"></a>环境收集的信息  
 对于每个网络计算机或你指定的服务器，日志收集收集以下环境的信息，并将其放入集锦日志文件。  
  
-   版本操作系统  
  
-   CPU 制造商和说明  
  
-   内存量和分配  
  
-   TCP/IP 绑定的网络适配器  
  
-   区域设置  
  
-   进程  
  
-   存储配置  
  
-   主机文件信息  
  
-   应用程序、 系统、 Windows Server 和 Media Center 包括事件日志  
  
-   服务控制管理器邮件  
  
-   重新启动事件和 Windows 更新事件  
  
-   系统错误和应用程序错误  
  
## <a name="services-information-collected"></a>服务收集的信息  
  
### <a name="server-services"></a>服务器服务  
  
-   Windows Server 客户端计算机备份服务  
  
-   Windows Server 客户端计算机备份提供商的服务  
  
-   Windows Server 设备提供程序  
  
-   Windows Server 域名称管理  
  
-   Windows Server 服务提供商注册表  
  
-   Windows Server 设置提供程序  
  
-   Windows Server UPnP 设备服务  
  
-   Windows 远程网站的访问权限管理提供程序  
  
-   Windows Server 运行状况服务  
  
-   Windows Server 存储服务  
  
-   Windows Server SQM 服务  
  
### <a name="network-computer-services"></a>网络计算机服务  
  
-   Windows Server 客户端计算机备份提供商的服务  
  
-   Windows Server 运行状况服务  
  
-   Windows Server 服务提供商注册表  
  
-   Windows Server SQM 服务  
  
## <a name="logs-and-registry-information-collected"></a>日志和注册表收集的信息  
 对于每个网络计算机或指定服务器，日志收集日志和注册表从收集信息服务器和网络的计算机，如下所示。  
  
### <a name="server-logs-and-registry-information"></a>服务器日志和注册表信息  
  
-   来自 < ProgramData\ > \Microsoft\Windows Server\Logs 服务器产品日志  
  
-   计划的任务  
  
-   安装 API 日志  
  
-   Windows 更新日志  
  
-   健康警报文件  
  
-   设备信息文件  
  
-   服务器备份日志文件  
  
-   黑豹日志文件  
  
-   服务  
  
-   注册表项从  
  
    -   \\\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\  
  
    -   \\\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DevicesProviderSvc  
  
    -   \\\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DomainManagerProviderSvc  
  
### <a name="network-computer-logs-and-registry-information"></a>网络计算机日志和注册表信息  
  
-   \Microsoft\Windows Server\Logs < ProgramData\ > 在网络计算机产品日志  
  
-   在 < ProgramData\ > \Microsoft\Windows Server\Data 健康警报文件  
  
-   Windows 更新日志  
  
-   安装 API 日志  
  
-   计划的任务信息  
  
-   从 \\\HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows Server\ 注册表项  
  
## <a name="logs-for-computers-that-do-not-run-a-version-of-the-windows-operating-system"></a>无法运行的 Windows 操作系统版本的计算机的日志  
 从计算机中并非运行的 Windows 操作系统版本日志收集不会收集日志文件。 对于非 Windows 的计算机，手动以下日志文件复制到同一个位置存储收集日志文件。  
  
-   System.log  
  
-   库/日志/Windows Server.log  
  
-   库/日志/CrashReporter/启动栏-< nnn\ > （复制的所有.crash 文件启动栏-< nnn\ >）  
  
-   库/日志/DiagnosticReports/启动栏-< nnn\ > （复制的所有.crash 文件启动栏-< nnn\ >）  
  
## <a name="see-also"></a>请参阅  
  

-   [日志收集错误疑难解答](Troubleshoot-Windows-Server-Essentials-Log-Collector-Errors.md)

-   [日志收集错误疑难解答](../support/Troubleshoot-Windows-Server-Essentials-Log-Collector-Errors.md)

