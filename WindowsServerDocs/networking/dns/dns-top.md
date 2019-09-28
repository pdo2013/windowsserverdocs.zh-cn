---
title: 域名系统 (DNS)
description: 本主题概述了 Windows Server 2016 中的 DNS
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: 1324ba18-4e28-4b9d-bbe7-75707e6d30ab
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6ad3b66ff0b271c3b6f6134a96aaf6b5171bc7d4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406169"
---
# <a name="domain-name-system-dns"></a>域名系统 (DNS)

>适用于：Windows Server（半年频道）、Windows Server 2016

域名系统（DNS）是包含 TCP/IP 的一套行业标准协议套件，同时 DNS 客户端和 DNS 服务器为计算机和用户提供计算机名称到 IP 地址的映射名称解析服务。  
  
> [!NOTE]  
> 除了本主题之外，还提供了以下 DNS 内容。  
>   
> -   [DNS 客户端中的新增功能](What-s-New-in-DNS-Client.md)  
> -   [DNS 服务器中的新增功能](What-s-New-in-DNS-Server.md)  
> -   [DNS 策略方案指南](deploy/DNS-Policy-Scenario-Guide.md)  
> -   视频：[Windows Server 2016：IPAM @ no__t 中的 DNS 管理  
  
在 Windows Server 2016 中，DNS 是可以使用服务器管理器或 Windows PowerShell 命令安装的服务器角色。 如果要安装新的 Active Directory 林和域，则 DNS 会自动安装 Active Directory 作为林和域的全局目录服务器。  
  
Active Directory 域服务（AD DS）使用 DNS 作为其域控制器定位机制。 当执行任何主体 Active Directory 操作（如身份验证、更新或搜索）时，计算机将使用 DNS 查找 Active Directory 域控制器。 此外，域控制器还使用 DNS 查找对方。  
  
DNS 客户端服务包括在 Windows 操作系统的所有客户端和服务器版本中，默认情况下，在操作系统安装时运行。 使用 DNS 服务器的 IP 地址配置 TCP/IP 网络连接时，DNS 客户端将查询 DNS 服务器以发现域控制器，并将计算机名称解析为 IP 地址。 例如，如果具有 Active Directory 用户帐户的网络用户登录到 Active Directory 域，则 DNS 客户端服务会查询 DNS 服务器以查找 Active Directory 域的域控制器。 如果 DNS 服务器响应查询并向客户端提供域控制器的 IP 地址，则客户端将与域控制器联系，并且身份验证过程就可以开始了。  
  
Windows Server 2016 DNS 服务器和 DNS 客户端服务使用 TCP/IP 协议组中包含的 DNS 协议。 DNS 是 TCP/IP 参考模型的应用层的一部分，如下图所示。  
  
![TCP/IP 中的 DNS](../media/Domain-Name-System--DNS-/dns_in_tcpip.jpg)  
  

