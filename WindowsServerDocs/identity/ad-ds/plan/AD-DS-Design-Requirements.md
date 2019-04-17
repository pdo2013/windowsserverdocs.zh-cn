---
ms.assetid: f6e76ef0-2217-4cdb-980f-22a780a85ebb
title: "广告 DS 设计要求"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 17338cd00fecec098865095dd9613f62beb3a457
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="ad-ds-design-requirements"></a>广告 DS 设计要求

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

  
## <a name="designing-the-active-directory-logical-structure"></a>设计 Active Directory 逻辑结构  
部署 Windows Server 2008 Active Directory 域服务 (广告 DS) 之前，你必须规划，并为您的环境的设计的广告 DS 逻辑结构。 广告 DS 逻辑结构确定 directory 对象的组织方式，并提供有关管理你的网络帐户和共享的资源有效方法。 在设计你广告 DS 逻辑结构时，你定义你的组织的网络基础结构的一个重要组成部分。  
  
设计广告 DS 逻辑结构，确定你的组织，它需要森林的数量，并创建设计域、 域名系统 (DNS) 的基础结构和单位 （华丽绚烂）。 下图显示设计逻辑结构的过程。  
  
![广告 DS 设计要求](media/AD-DS-Design-Requirements/d5cebae6-a752-4063-a98f-473799c251bd.gif)  
  
有关详细信息，请参阅[设计逻辑结构为 Windows Server 2008 广告 DS](Designing-the-Logical-Structure.md)。  
  
## <a name="designing-the-site-topology"></a>设计网站拓扑  
你为你的广告 DS 基础结构设计逻辑结构后，你必须为你的网络设计站点拓扑。 站点拓扑逻辑代表你的物理网络。 它包含有关广告 DS 站点，每个站点，并站点链接和网站的链接桥支持站点之间的广告 DS 复制内的广告 DS 域控制器的位置信息。 下图显示的网站拓扑设计过程。  
  
![广告 DS 设计要求](media/AD-DS-Design-Requirements/d34d43c0-437f-47cb-9b64-09c0f9ce6479.gif)  
  
有关详细信息，请参阅[设计网站拓扑为 Windows Server 2008 广告 DS](Designing-the-Site-Topology.md)。  
  
## <a name="planning-domain-controller-capacity"></a>计划域控制器容量  
若要确保高效广告 DS 性能，必须确定相应每个网站的域控制器的数量，并验证它们满足 Windows Server 2008 的硬件要求。 仔细容量域控制器规划确保不要低估硬件要求，可能会导致差域控制器应用程序的性能和响应时间。 下图显示域控制器容量规划的过程。  
  
![广告 DS 设计要求](media/AD-DS-Design-Requirements/fff6ef22-5c7b-4478-ad76-42b296dcf769.gif)  
  
## <a name="enabling-windows-server-2008-advanced-ad-ds-features"></a>启用 Windows Server 2008 高级广告 DS 功能  
你可以使用 Windows Server 2008 广告 DS 通过提升域中，还是森林功能级别引入到您的环境的高级的功能。 域或森林中的所有域控制器在都运行 Windows Server 2008 时，你可以提升对 Windows Server 2008 功能级别。  
  
有关详细信息，请参阅[启用高级功能广告 DS](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md)。  
  


