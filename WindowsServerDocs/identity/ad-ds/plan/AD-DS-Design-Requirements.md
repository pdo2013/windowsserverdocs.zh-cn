---
ms.assetid: f6e76ef0-2217-4cdb-980f-22a780a85ebb
title: AD DS 设计要求
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: cb9d4c04bc3fc7bb534e75b80f0947bd225b7b85
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71368959"
---
# <a name="ad-ds-design-requirements"></a>AD DS 设计要求

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

  
## <a name="designing-the-active-directory-logical-structure"></a>设计 Active Directory 逻辑结构  
在部署 Windows Server 2008 Active Directory 域服务（AD DS）之前，必须规划并设计环境的 AD DS 逻辑结构。 AD DS 逻辑结构确定目录对象的组织方式，并提供有效的方法来管理网络帐户和共享资源。 设计 AD DS 逻辑结构时，可以定义组织的网络基础结构的一个重要部分。  
  
若要设计 AD DS 逻辑结构，请确定你的组织需要的林数量，然后创建域、域名系统（DNS）基础结构和组织单位（Ou）的设计。 下图显示了设计逻辑结构的过程。  
  
![AD DS 设计要求](media/AD-DS-Design-Requirements/d5cebae6-a752-4063-a98f-473799c251bd.gif)  
  
有关详细信息，请参阅[设计 Windows Server 2008 的逻辑结构 AD DS](Designing-the-Logical-Structure.md)。  
  
## <a name="designing-the-site-topology"></a>设计站点拓扑  
为 AD DS 基础结构设计逻辑结构后，必须为网络设计站点拓扑。 站点拓扑是物理网络的逻辑表示形式。 它包含有关 AD DS 站点的位置、每个站点内的 AD DS 域控制器的信息，以及支持站点之间 AD DS 复制的站点链接和站点链接桥。 下图显示了站点拓扑设计过程。  
  
![AD DS 设计要求](media/AD-DS-Design-Requirements/d34d43c0-437f-47cb-9b64-09c0f9ce6479.gif)  
  
有关详细信息，请参阅[设计适用于 Windows Server 2008 的站点拓扑 AD DS](Designing-the-Site-Topology.md)。  
  
## <a name="planning-domain-controller-capacity"></a>规划域控制器容量  
为了确保高效 AD DS 性能，你必须为每个站点确定适当数量的域控制器，并验证它们是否满足 Windows Server 2008 的硬件要求。 对域控制器进行周密的容量规划可确保不会低估硬件要求，这可能会导致域控制器性能和应用程序响应时间不佳。 下图显示了域控制器容量规划过程。  
  
![AD DS 设计要求](media/AD-DS-Design-Requirements/fff6ef22-5c7b-4478-ad76-42b296dcf769.gif)  
  
## <a name="enabling-windows-server-2008-advanced-ad-ds-features"></a>启用 Windows Server 2008 advanced AD DS 功能  
你可以通过提高域或林功能级别，使用 Windows Server 2008 AD DS 向环境引入高级功能。 当域或林中的所有域控制器都运行 Windows Server 2008 时，可以将功能级别提升到 Windows Server 2008。  
  
有关详细信息，请参阅[启用 AD DS 的高级功能](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md)。  
  


