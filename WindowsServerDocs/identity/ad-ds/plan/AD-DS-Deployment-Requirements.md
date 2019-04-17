---
ms.assetid: e02bb152-d0db-40b0-9942-846dce75f6c7
title: "广告 DS 部署的要求。"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7edd7de8077a077245416f859838a6bc55415edc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="ad-ds-deployment-requirements"></a>广告 DS 部署的要求。

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

你的现有环境结构确定部署 Windows Server 2008 Active Directory 域服务 (广告 DS) 的策略。 如果您没有现有域结构要创建广告 DS 环境，完成广告 DS 设计，然后再开始创建广告 DS 环境。 然后，你可以部署新林根域，并根据你的设计部署域结构的其余部分。  
  
此外，部分广告 DS 部署，你可能决定升级，并重新组织你环境。 例如，如果你的组织有现有的 Windows 2000 域结构，你可能会执行的一些域就地升级，并重新构建其他人。 此外，你可能决定，减少了您的环境的复杂性重构林之间的域，或者后部署广告 DS 重构林中的域。  
  
## <a name="deploying-a-windows-server-2008-forest-root-domain"></a>部署 Windows Server 2008 森林根域  
森林根域奠定基础有关您的广告 DS 森林基础结构。 部署广告 DS，必须首先部署森林根域。 若要执行此操作，你必须查看您的广告 DS 设计;配置森林根域; DNS 服务创建森林根域，包括部署森林根域控制器、 配置森林根域的站点拓扑和配置操作主机角色 （也称为灵活一个主机操作或 FSMO）;并引发了林和域功能级别。 下图显示部署森林根域整个的过程。  
  
![广告 DS 要求](media/AD-DS-Deployment-Requirements/033aad0b-25ff-4793-8825-88a6daa01a55.gif)  
  
有关详细信息，请参阅[部署 Windows Server 2008 森林根域](https://technet.microsoft.com/library/cc731174.aspx)。  
  
## <a name="deploying-windows-server-2008-regional-domains"></a>部署 Windows Server 2008 区域  
完成森林根域部署后，你就可以部署任何新的 Windows Server 2008 区域域，由你的设计。 若要执行此操作，你必须部署域控制器，以便每个区域的域。 下图显示部署区域的域的过程。  
  
![广告 DS 要求](media/AD-DS-Deployment-Requirements/89a878c8-9a94-4180-ad43-ca75316a6318.gif)  
  
有关详细信息，请参阅[部署 Windows Server 2008 区域域](https://technet.microsoft.com/library/cc755118.aspx)。  
  
## <a name="upgrading-active-directory-domains-to-windows-server-2008"></a>升级到 Windows Server 2008 Active Directory 域  
对 Windows Server 2008 域升级你的 Windows 2000 或 Windows Server 2003 域是充分利用 Windows Server 2008 的更多特性和功能高效、 简单方法。 你可以升级域维护你当前的网络和域配置同时改进了安全性、 可扩展性和可管理性网络基础结构。 从 Windows 2000 或 Windows Server 2003 升级到 Windows Server 2008 要求最少的网络配置。 升级还有几乎影响上用户的操作。 有关详细信息，请参阅[对 Windows Server 2008 和 Windows Server 2008 R2 广告 DS 域升级 Active Directory 域](https://technet.microsoft.com/library/cc731188.aspx)。  
  
## <a name="restructuring-ad-ds-domains"></a>重新构造广告 DS 域  
当你重新构建 Windows Server 2008 林 （林间重构） 之间的域时，你可以减少域数您的环境中，并因此减少管理复杂性和开销。 迁移林此过程中之间的对象重构过程，同时存在源代码域和目标域环境。 这使你可以回退到源环境期间迁移，如有必要。  
  
当你重新构建 Windows Server 2008 域，Windows Server 2008 树林 （内联目录林重构） 中的时，你可以整合域结构和因此减少管理复杂性和开销。 当你重新构建林中域时，源域中不会再存在迁移的帐户。  
  
有关如何使用 Active Directory 迁移工具 (ADMT) 3.1 （ADMT 3.1 版） 重新构建域的详细信息，请参阅[ADMT 3.1 版迁移指南](https://go.microsoft.com/fwlink/?LinkId=93678)。  
  


