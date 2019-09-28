---
title: 步骤2规划群集服务器
description: 本主题是在 Windows Server 2016 的群集中部署远程访问指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 673c5bfb-b590-4932-8e54-ca0a466d90cc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 17aadbb789052be7f33822ce49f3b797f2211d55
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367377"
---
# <a name="step-2-plan-cluster-servers"></a>步骤2规划群集服务器

>适用于：Windows Server（半年频道）、Windows Server 2016

部署单个远程访问服务器后，请计划将其他服务器添加到群集。  
  
|任务|描述|  
|----|--------|  
|[2.1 安装角色和功能](#BKMK_Install)。|对于将添加到群集中的每个服务器，计划安装远程访问角色和 Windows NLB 功能（如果需要），规划拓扑、IP 寻址、路由和转发。|  
|[2.2 配置服务器设置](#BKMK_Config)|配置将添加到群集中的每个服务器的设置。 请注意，可以使用虚拟机配置服务器的负载均衡群集。 为了使路由和连接正常工作，必须将虚拟机配置为使用 MAC 地址欺骗。|  
  
## <a name="BKMK_Install"></a>2.1 安装角色和功能  
对于要加入群集的每个服务器，请计划安装远程访问角色。 此外，如果你想要使用 Windows NLB 对流量进行负载均衡，请计划安装网络负载平衡（NLB）功能。 有关详细信息，请参阅[网络负载平衡](https://technet.microsoft.com/windows-server-docs/networking/technologies/network-load-balancing)。  
  
## <a name="BKMK_Config"></a>2.2 配置服务器设置  
对于将添加到群集的每个服务器，请规划 IP 地址和域设置。 请注意以下事项：  
  
1.  群集中的所有服务器都必须属于同一个域。  
  
2.  群集中的服务器必须位于同一子网中。  
  
3.  群集中的每个服务器都应有相同数量的网络适配器用于 DirectAccess 部署。  
  
使用 Windows NLB 对群集进行负载平衡时，将应用以下 Windows NLB 设置：  
  
1.  操作模式-单播。 这可以使用 NLB 管理器更改为多播。 此设置不能在远程访问管理控制台中进行修改。  
  
2.  负载权重系数定义为相等，其中所有群集服务器的负载相等。  
  
3.  筛选模式-将在多个主机之间对流量进行负载均衡。  
  
4.  关联-已定义单个关联。  
  
5.  协议-两者  

