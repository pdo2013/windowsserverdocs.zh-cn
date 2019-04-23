---
ms.assetid: e02bb152-d0db-40b0-9942-846dce75f6c7
title: AD DS 部署要求
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d21491f5ce9c15ecc514e4be24a91de28b0fd0ce
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890968"
---
# <a name="ad-ds-deployment-requirements"></a>AD DS 部署要求

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

在现有环境的结构决定你用于部署 Windows Server 2008 Active Directory 域服务 (AD DS) 的策略。 如果你要创建 AD DS 环境，并且不具有现有域结构，完成 AD DS 设计，才能开始创建你的 AD DS 环境。 然后，可以部署新林根级域，并根据您的设计部署您的域结构的其余部分。  
  
此外，作为一部分的 AD DS 部署，您可能决定升级和重新构建您的环境。 例如，如果你的组织具有现成的 Windows 2000 域结构，可能执行的某些域在就地升级并重组其他人。 此外，您可能决定通过重组林之间或林内的域林中部署 AD DS 后降低您的环境的复杂性。  
  
## <a name="deploying-a-windows-server-2008-forest-root-domain"></a>部署 Windows Server 2008 目录林根域  
目录林根域的 AD DS 林基础结构提供了基础。 若要部署 AD DS，必须先部署目录林根域。 若要执行此操作，你必须查看你的 AD DS 设计;配置为目录林根域; 的 DNS 服务创建目录林根域，其中包括部署林根级域控制器、 配置林根域的站点拓扑和配置操作主机角色 （也称为灵活单主机操作或 FSMO）;和提升林和域功能级别。 下图显示部署目录林根域的整个过程。  
  
![AD DS 要求](media/AD-DS-Deployment-Requirements/033aad0b-25ff-4793-8825-88a6daa01a55.gif)  
  
有关详细信息，请参阅[部署 Windows Server 2008 目录林根域](https://technet.microsoft.com/library/cc731174.aspx)。  
  
## <a name="deploying-windows-server-2008-regional-domains"></a>部署 Windows Server 2008 地区域  
林根级域部署完成后，你现可部署任何新 Windows Server 2008 地区域所指定的设计。 若要执行此操作，必须部署为每个地区域的域控制器。 下图显示了部署地区域的过程。  
  
![AD DS 要求](media/AD-DS-Deployment-Requirements/89a878c8-9a94-4180-ad43-ca75316a6318.gif)  
  
有关详细信息，请参阅[部署 Windows Server 2008 地区域](https://technet.microsoft.com/library/cc755118.aspx)。  
  
## <a name="upgrading-active-directory-domains-to-windows-server-2008"></a>将 Active Directory 域升级到 Windows Server 2008  
升级到 Windows Server 2008 域的 Windows 2000 或 Windows Server 2003 域是一种有效简单的方式来利用其他 Windows Server 2008 特性和功能。 你可以升级域来维护当前的网络和域配置同时提高安全性、 可伸缩性和可管理性的网络基础结构。 从 Windows 2000 或 Windows Server 2003 升级到 Windows Server 2008 需要最少的网络配置。 升级还具有对用户操作几乎没有影响。 有关详细信息，请参阅[到 Windows Server 2008 和 Windows Server 2008 R2 AD DS 域升级 Active Directory 域](https://technet.microsoft.com/library/cc731188.aspx)。  
  
## <a name="restructuring-ad-ds-domains"></a>重新构建 AD DS 域  
重组 Windows Server 2008 林 （林间重组） 之间的域时, 可以减少您的环境中的域，并因此降低管理复杂性和开销。 当您迁移对象的这一部分的林之间重构过程时，源域和目标域环境同时存在。 这使得你可以回滚到源环境在迁移期间，如有必要。  
  
重组林内部的 Windows Server 2008 （林内重组） 的 Windows Server 2008 域，可以合并域结构，并因此降低管理复杂性和开销。 重组林内部的域时, 已迁移的帐户将不再存在的源域中。  
  
有关如何使用 Active Directory 迁移工具 (ADMT) 版本 3.1 (ADMT v3.1) 重组域的详细信息，请参阅[ADMT v3.1 迁移指南](https://go.microsoft.com/fwlink/?LinkId=93678)。  
  


