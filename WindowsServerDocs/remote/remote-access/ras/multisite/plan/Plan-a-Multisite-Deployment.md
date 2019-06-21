---
title: 规划多站点部署
description: 本主题是指南的一部分部署多台远程访问服务器在 Windows Server 2016 中的多站点部署中。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8387eabe-7363-4367-b5b1-03c67baa2933
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ba813fc5da53e9635e9ef1363f1a559a29419312
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282603"
---
# <a name="plan-a-multisite-deployment"></a>规划多站点部署

>适用于：Windows 服务器 （半年频道），Windows Server 2016

 Windows Server 2016 和 Windows Server 2012 将 DirectAccess 以及路由和远程访问服务 (RRAS) VPN 合并到单个远程访问角色。 本概述介绍以便在多站点配置中部署 Windows Server 2016 或 Windows Server 2012 远程访问所需的规划步骤。  
  
1.  [部署单个 DirectAccess 服务器使用高级设置](https://technet.microsoft.com/library/hh831436(v=ws.11).aspx)。 此步骤包括规划部署一台服务器所需的基础结构。 它包括规划网络和服务器设置、 证书要求、 DNS 设置、 网络位置服务器部署、 DirectAccess 管理服务器、 Active Directory 设置和组策略对象 (Gpo)。  
  
2.  [步骤 2 规划多站点基础结构](Step-2-Plan-the-Multisite-Infrastructure.md)。 此步骤包括 Active Directory 和 GPO 规划，以及 DNS 配置。  
  
3.  [步骤 3 规划多站点部署](Step-3-Plan-the-Multisite-Deployment.md)。 此步骤包括规划证书设置、 网络位置服务器配置、 客户端入口点设置、 IPv6 前缀设置和根据需要全局负载平衡设置。  
  
> [!NOTE]  
> 记录用于远程访问高级部署规划决策。 对于完成部署步骤所涉及的每个人，此记录可用来帮助进行工作。  
  
完成这些规划步骤后，请参阅[配置多站点部署](../configure/Configure-a-Multisite-Deployment.md)。  
  


