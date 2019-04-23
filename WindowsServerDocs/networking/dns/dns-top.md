---
title: 域名系统 (DNS)
description: 本主题提供 Windows Server 2016 中 DNS 的概述
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 1324ba18-4e28-4b9d-bbe7-75707e6d30ab
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3d4ec63e904dd899a3ddc53a59274ad607136edd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870818"
---
# <a name="domain-name-system-dns"></a>域名系统 (DNS)

>适用于：Windows 服务器 （半年频道），Windows Server 2016

域名系统 (DNS) 是构成 TCP/IP 的协议的行业标准套件之一，DNS 客户端和 DNS 服务器一起提供的计算机和用户的计算机名称到 IP 地址映射名称解析服务。  
  
> [!NOTE]  
> 除了本主题中，下面的 DNS 内容可用。  
>   
> -   [什么是 DNS 客户端中的新增功能](What-s-New-in-DNS-Client.md)  
> -   [什么是 DNS 服务器中的新增功能](What-s-New-in-DNS-Server.md)  
> -   [DNS 策略方案指南](deploy/DNS-Policy-Scenario-Guide.md)  
> -   视频：[Windows Server 2016:在 IPAM 中的 DNS 管理](https://channel9.msdn.com/Blogs/windowsserver/Windows-Server-2016-DNS-management-in-IPAM)  
  
Windows Server 2016 中 DNS 是可以使用服务器管理器或 Windows PowerShell 命令安装的服务器角色。 如果您正在安装新的 Active Directory 林和域，DNS 会自动安装与 Active Directory 作为林和域的全局目录服务器。  
  
Active Directory 域服务 (AD DS) 作为其域控制器位置机制使用 DNS。 任何主体的 Active Directory 操作执行时，例如身份验证、 更新或搜索，计算机使用 DNS 来查找 Active Directory 域控制器。 此外，域控制器使用 DNS 找到对方。  
  
DNS 客户端服务包含在所有客户端和服务器版本的 Windows 操作系统中，在操作系统安装时默认情况下运行。 在 DNS 服务器的 IP 地址配置 TCP/IP 网络连接时，DNS 客户端查询 DNS 服务器以发现域控制器，并解析为 IP 地址的计算机名称。 例如，网络用户使用 Active Directory 用户帐户登录到 Active Directory 域，DNS 客户端服务查询以查找 Active Directory 域的域控制器的 DNS 服务器。 DNS 服务器响应查询，并向客户端提供域控制器的 IP 地址，客户端会联系域控制器，可以开始身份验证过程。  
  
在 Windows Server 2016 DNS 服务器和 DNS 客户端服务使用的 TCP/IP 协议套件中包含的 DNS 协议。 DNS 是 TCP/IP 参考模型，应用程序层的一部分，如以下插图所示。  
  
![TCP/IP 中的 DNS](../media/Domain-Name-System--DNS-/dns_in_tcpip.jpg)  
  

