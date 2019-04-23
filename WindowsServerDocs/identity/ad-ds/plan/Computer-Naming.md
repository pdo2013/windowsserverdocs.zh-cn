---
ms.assetid: f7002265-60fa-40b8-9dd7-4bf131d9320a
title: 计算机命名
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ae2483571d67b4cdb32c2a547b924b1315573da0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842158"
---
# <a name="computer-naming"></a>计算机命名

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

当运行 Windows 2000，Windows XP、 Windows Server 2003、 Windows Server 2008 或 Windows Vista 操作系统的计算机加入域，默认情况下在计算机为自己分配一个名称。 它为自己分配的名称包含计算机 （即，系统属性的计算机名称） 的主机名和将计算机加入的 Active Directory 域的域名系统 (DNS) 名称 （即，在系统属性中主 DNS 后缀）。 主机名和域的 DNS 名称的串联称为完全限定的域名 (FQDN)。 例如，如果与主机的计算机名称 Server1 联接域 corp.contoso.com，计算机的 FQDN 是 server1.corp.contoso.com。  
  
如果计算机已具有不同 DNS 域名的静态 DNS 区域中输入或注册的集成的 DNS/动态主机配置协议 (DHCP) 服务器服务，计算机的 FQDN 是不同于已注册的名称以前。 可以按任一名称引用计算机。  
  
有关 Active Directory 域服务 (AD DS) 中的命名约定的详细信息，请参阅 Active Directory 中的计算机、 域、 站点和组织单位的命名约定 ([https://go.microsoft.com/fwlink/?LinkID=106629](https://go.microsoft.com/fwlink/?LinkID=106629))。  
  


