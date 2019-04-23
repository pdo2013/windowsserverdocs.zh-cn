---
ms.assetid: f6e76ef0-2217-4cdb-980f-22a780a85ebb
title: AD DS 设计要求
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 79c694112f39adf5d37cd28f6bd7a770dedf3976
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857518"
---
# <a name="ad-ds-design-requirements"></a>AD DS 设计要求

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

  
## <a name="designing-the-active-directory-logical-structure"></a>设计 Active Directory 逻辑结构  
在部署 Windows Server 2008 Active Directory 域服务 (AD DS) 之前，必须规划和设计适用于你环境的 AD DS 逻辑结构。 AD DS 逻辑结构决定目录对象的组织方式，并提供一种有效方法用于管理你的网络帐户和共享的资源。 在设计您的 AD DS 逻辑结构时，您定义您的组织的网络基础结构的重要组成部分。  
  
若要设计 AD DS 逻辑结构，确定你的组织要求的林数量，并创建为域、 域名系统 (DNS) 基础结构和组织单位 (Ou) 的设计。 下图显示了设计逻辑结构的过程。  
  
![AD DS 设计要求](media/AD-DS-Design-Requirements/d5cebae6-a752-4063-a98f-473799c251bd.gif)  
  
有关详细信息，请参阅[设计逻辑结构的 Windows Server 2008 AD DS](Designing-the-Logical-Structure.md)。  
  
## <a name="designing-the-site-topology"></a>设计站点拓扑  
为 AD DS 基础结构设计逻辑结构后，必须为你的网络设计站点拓扑。 站点拓扑是在物理网络的逻辑表示形式。 它包含有关 AD DS 站点，每个站点的站点链接和支持站点之间的 AD DS 复制的站点链接桥中的 AD DS 域控制器位置的信息。 下图显示了站点拓扑设计过程。  
  
![AD DS 设计要求](media/AD-DS-Design-Requirements/d34d43c0-437f-47cb-9b64-09c0f9ce6479.gif)  
  
有关详细信息，请参阅[设计站点拓扑的 Windows Server 2008 AD DS](Designing-the-Site-Topology.md)。  
  
## <a name="planning-domain-controller-capacity"></a>规划域控制器容量  
若要确保高效 AD DS 的性能，必须确定适当数量的每个站点的域控制器，并验证它们满足 Windows Server 2008 的硬件要求。 规划你的域控制器进行细致的能力可确保不低估硬件要求，可能会导致不佳的域控制器性能和应用程序响应时间。 下图显示域控制器容量规划的过程。  
  
![AD DS 设计要求](media/AD-DS-Design-Requirements/fff6ef22-5c7b-4478-ad76-42b296dcf769.gif)  
  
## <a name="enabling-windows-server-2008-advanced-ad-ds-features"></a>启用 Windows Server 2008 高级 AD DS 功能  
可以使用 Windows Server 2008 AD DS 域或林功能级别提升高级的功能引入你的环境。 当域或林中的所有域控制器都运行 Windows Server 2008 时，你可以将提升到 Windows Server 2008 的功能级别。  
  
有关详细信息，请参阅[适用于 AD DS 启用高级功能](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md)。  
  


