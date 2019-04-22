---
ms.assetid: c32606b4-2ee2-4df3-a704-8ac6723e188f
title: DNS 和 AD DS
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f6d75a78119d76a0f8380967292b1d0abc720597
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813138"
---
# <a name="dns-and-ad-ds"></a>DNS 和 AD DS

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

Active Directory 域服务 (AD DS) 使用域名系统 (DNS) 名称解析服务使客户端查找域控制器和域控制器承载目录服务进行相互通信。  
  
AD DS 更方便地集成到现有的 DNS 命名空间的 Active Directory 命名空间。 功能如 Active Directory 集成的 DNS 区域使您更轻松地将 DNS 部署，无需设置辅助区域，然后配置区域传送。  
  
DNS 如何支持 AD DS 有关的信息，请参阅明[Active Directory 技术参考的 DNS 支持](https://go.microsoft.com/fwlink/?LinkID=48147)。  
  
> [!NOTE]  
> 如果你实现的 AD DS 的域名与客户端使用的主 DNS 后缀非连续命名空间，使用 DNS 的 AD DS 集成是更复杂。 有关详细信息，请参阅[不连续的 Namespace](../../ad-ds/plan/../../ad-ds/plan/Disjoint-Namespace.md)。  
  
## <a name="in-this-section"></a>本节内容  
  
- [域控制器位置](../../ad-ds/plan/Domain-Controller-Location.md)  
- [Active Directory 集成 DNS 区域](../../ad-ds/plan/Active-Directory-Integrated-DNS-Zones.md)  
- [计算机命名](../../ad-ds/plan/Computer-Naming.md)  
- [不连续的 Namespace](../../ad-ds/plan/../../ad-ds/plan/Disjoint-Namespace.md)  
