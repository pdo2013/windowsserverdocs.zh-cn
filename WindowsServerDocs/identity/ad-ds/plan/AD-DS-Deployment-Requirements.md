---
ms.assetid: e02bb152-d0db-40b0-9942-846dce75f6c7
title: AD DS 部署要求
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: a23ba4ac8bbdb076381c8419e3a0821bee364acf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71409030"
---
# <a name="ad-ds-deployment-requirements"></a>AD DS 部署要求

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

现有环境的结构确定了部署 Windows Server 2008 Active Directory 域服务（AD DS）的策略。 如果正在创建 AD DS 环境，但没有现有的域结构，请在开始创建 AD DS 环境之前完成 AD DS 设计。 然后，你可以部署新的目录林根级域，并根据你的设计部署域结构的其余部分。  
  
另外，作为 AD DS 部署的一部分，你可能决定升级并重构你的环境。 例如，如果你的组织具有现有的 Windows 2000 域结构，你可以执行某些域的就地升级，并重构其他域。 此外，你可能会决定在部署 AD DS 后，通过在林之间重建域或在林中重新构建域，来降低环境的复杂性。  
  
## <a name="deploying-a-windows-server-2008-forest-root-domain"></a>部署 Windows Server 2008 林根级域  
目录林根级域提供 AD DS 林基础结构的基础。 若要部署 AD DS，你必须首先部署目录林根级域。 为此，必须查看 AD DS 设计;为目录林根级域配置 DNS 服务;创建目录林根级域，其中包括部署目录林根级域控制器、为目录林根级域配置站点拓扑，以及配置操作主机角色（也称为灵活单主机操作或 FSMO）;并提升林和域功能级别。 下图显示了部署目录林根级域的整个过程。  
  
![AD DS 要求](media/AD-DS-Deployment-Requirements/033aad0b-25ff-4793-8825-88a6daa01a55.gif)  
  
有关详细信息，请参阅[部署 Windows Server 2008 林根级域](https://technet.microsoft.com/library/cc731174.aspx)。  
  
## <a name="deploying-windows-server-2008-regional-domains"></a>部署 Windows Server 2008 地区性域  
完成目录林根级域的部署后，便可以部署设计所指定的任何新的 Windows Server 2008 地区性域。 为此，你必须为每个地区性域部署域控制器。 下图显示了部署区域的过程。  
  
![AD DS 要求](media/AD-DS-Deployment-Requirements/89a878c8-9a94-4180-ad43-ca75316a6318.gif)  
  
有关详细信息，请参阅[部署 Windows Server 2008 地区性域](https://technet.microsoft.com/library/cc755118.aspx)。  
  
## <a name="upgrading-active-directory-domains-to-windows-server-2008"></a>将 Active Directory 域升级到 Windows Server 2008  
将 Windows 2000 或 Windows Server 2003 域升级到 Windows Server 2008 域是利用附加 Windows Server 2008 特性和功能的一种高效、直接的方法。 可以升级域以维护当前网络和域配置，同时提高网络基础结构的安全性、可伸缩性和可管理性。 从 Windows 2000 或 Windows Server 2003 升级到 Windows Server 2008 需要最少的网络配置。 升级还对用户操作影响很小。 有关详细信息，请参阅[将 Active Directory 域升级到 Windows server 2008 和 Windows server 2008 R2 AD DS 域](https://technet.microsoft.com/library/cc731188.aspx)。  
  
## <a name="restructuring-ad-ds-domains"></a>重构 AD DS 域  
当你在 Windows Server 2008 林（林间重组）之间重建域时，你可以减少环境中的域数量，从而降低管理复杂性和开销。 在此重构过程中，当你在林之间迁移对象时，源域和目标域环境同时存在。 这使你在迁移过程中可以在迁移过程中回滚到源环境（如有必要）。  
  
当你在 Windows Server 2008 林（林内重构）中重构 Windows Server 2008 域时，可以合并你的域结构，从而降低管理复杂性和开销。 重构林中的域时，已迁移的帐户将不再存在于源域中。  
  
有关如何使用 Active Directory 迁移工具（ADMT）3.1 版（ADMT 版3.1）来重新构建域的详细信息，请参阅[ADMT 3.1 版迁移指南](https://go.microsoft.com/fwlink/?LinkId=93678)。  
  


