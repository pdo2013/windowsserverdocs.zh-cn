---
ms.assetid: f7002265-60fa-40b8-9dd7-4bf131d9320a
title: "计算机命名"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d0dbe2da76a1cf3d1a4dd74183b5dd7106ef17b9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="computer-naming"></a>计算机命名

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

当计算机运行的 Windows 2000，Windows XP 中，Windows Server 2003，Windows Server 2008 或 Windows Vista 的操作系统加入域，默认情况下在计算机命名本身。 它将自行分配的名称包含计算机（即，在系统属性计算机名称）主机名称和计算机加入的 Active Directory 域域名系统 (DNS) 名称（即，在系统属性主 DNS 职务）。 连接的主名称和 DNS 域名称被称为完整的域名 (FQDN)。 例如，如果主机名称服务器 1 计算机加入的域 corp.contoso.com 时，计算机 FQDN 是 server1.corp.contoso.com。  
  
如果计算机已经有不同的 DNS 域名称静态输入 DNS 区域或由集成 DNS 注册 / 动态主机配置协议 (DHCP) 服务器服务，该计算机 FQDN 是截然不同之前已注册的名称。 可以通过任一名称中引用计算机。  
  
有关 Active Directory 域服务 (广告 DS) 中命名惯例的详细信息，请参阅命名约定 Active Directory 中计算机、域的站点和单位 ([https://go.microsoft.com/fwlink/?LinkID=106629](https://go.microsoft.com/fwlink/?LinkID=106629))。  
  


