---
title: 域名系统 (DNS)
description: 本主题提供 Windows Server 2016 中的 DNS 的概述
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 1324ba18-4e28-4b9d-bbe7-75707e6d30ab
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 23851d2d8015fc6ae9e0653e8a0843f8c4295162
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="domain-name-system-dns"></a>域名系统 (DNS)

>适用于：Windows Server（半年通道），Windows Server 2016

域名系统 (DNS) 是一符合行业标准套件一部分协议包含 TCP/IP，并在一起 DNS 客户端和 DNS 服务器计算机名称-TO-IP 地址映射名称分辨率为提供服务的计算机和用户。  
  
> [!NOTE]  
> 本主题中，除了以下 DNS 内容才可用。  
>   
> -   [什么是 DNS 客户端中的新增功能](What-s-New-in-DNS-Client.md)  
> -   [在 DNS 服务器中的新增功能](What-s-New-in-DNS-Server.md)  
> -   [DNS 策略方案指南](deploy/DNS-Policy-Scenario-Guide.md)  
> -   视频：[Windows Server 2016: IPAM 中的 DNS 管理](https://channel9.msdn.com/Blogs/windowsserver/Windows-Server-2016-DNS-management-in-IPAM)  
  
Windows Server 2016，DNS 是你可以通过使用服务器管理器或 Windows PowerShell 命令安装了服务器作用。 如果你安装了新的 Active Directory 森林和域，DNS 将自动安装使用 Active Directory 作为林和域的全球目录服务器。  
  
Active Directory 域服务(广告 DS) 使用 DNS 作为其域控制器位置机制。 任何主体的 Active Directory 操作执行时，如身份验证、更新，或搜索时，计算机使用 DNS 找不到 Active Directory 域控制器。 此外，域控制器使用 DNS 找到彼此。  
  
DNS 客户服务，包含在所有的客户端和服务器版本的 Windows 操作系统、和操作系统安装在默认情况下运行。 当您将配置 TCP/IP 网络连接 DNS 服务器的 IP 地址时，DNS 客户端查询 DNS 服务器发现域控制器，并解决计算机名为 IP 地址。 例如，当网络用户的 Active Directory 的用户帐户登录到 Active Directory 域，DNS 客户端服务查询以查找为域的 Active Directory 的域控制器 DNS 服务器。 当 DNS 服务器回复查询，并且为客户提供了域控制器 IP 地址时，的客户端联系域控制器，并且可以开始身份验证过程。  
  
Windows Server 2016 DNS 服务器和 DNS 客户端服务使用包含 tcp/ip 协议中的 DNS 协议。 DNS 是 TCP/IP 参考模型，应用程序层的一部分，在下图所示。  
  
![TCP/IP 中的 DNS](../media/Domain-Name-System--DNS-/dns_in_tcpip.jpg)  
  

