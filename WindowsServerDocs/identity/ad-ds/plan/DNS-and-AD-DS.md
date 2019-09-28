---
ms.assetid: c32606b4-2ee2-4df3-a704-8ac6723e188f
title: DNS 和 AD DS
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 82c96ac3f146510c5590aabea75a60ca0f5f90cc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402682"
---
# <a name="dns-and-ad-ds"></a>DNS 和 AD DS

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Active Directory 域服务（AD DS）使用域名系统（DNS）名称解析服务，使客户端能够查找域控制器和托管目录服务以便彼此通信的域控制器。  
  
AD DS 可以轻松地将 Active Directory 命名空间集成到现有的 DNS 命名空间中。 Active Directory 集成的 DNS 区域等功能使你可以更轻松地部署 DNS，因为无需设置辅助区域，然后配置区域传输。  
  
有关 DNS 如何支持 AD DS 的信息，请参阅[Active Directory 技术参考的 Dns 支持](https://go.microsoft.com/fwlink/?LinkID=48147)部分。  
  
> [!NOTE]  
> 如果实现了一个不连续的命名空间，其中 AD DS 域名不同于客户端使用的主 DNS 后缀，则与 DNS 的 AD DS 集成更为复杂。 有关详细信息，请参阅非[连续命名空间](../../ad-ds/plan/../../ad-ds/plan/Disjoint-Namespace.md)。  
  
## <a name="in-this-section"></a>本节内容  
  
- [域控制器位置](../../ad-ds/plan/Domain-Controller-Location.md)  
- [Active Directory 集成 DNS 区域](../../ad-ds/plan/Active-Directory-Integrated-DNS-Zones.md)  
- [计算机命名](../../ad-ds/plan/Computer-Naming.md)  
- [不连续的命名空间](../../ad-ds/plan/../../ad-ds/plan/Disjoint-Namespace.md)  
