---
title: 步骤 2 计划群集服务器
description: 本主题是指南的在 Windows Server 2016 中的群集中部署远程访问的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 673c5bfb-b590-4932-8e54-ca0a466d90cc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1f0ab7c1d888466ed575ab7cc2ed204db9a02542
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854408"
---
# <a name="step-2-plan-cluster-servers"></a>步骤 2 计划群集服务器

>适用于：Windows 服务器 （半年频道），Windows Server 2016

部署单一远程访问服务器之后, 计划向群集添加其他服务器。  
  
|任务|描述|  
|----|--------|  
|[2.1 安装角色和功能](#BKMK_Install)。|对于每个服务器将添加到群集，请规划远程访问角色和 Windows NLB 功能的安装 （如果需要），规划的拓扑、 IP 寻址、 路由和转发。|  
|[2.2 配置服务器设置](#BKMK_Config)|配置设置将添加到群集的每个服务器。 请注意，您可以配置负载平衡的群集使用的虚拟机的服务器。 为了使路由和连接才能正常工作，必须配置虚拟机使用 MAC 地址欺骗。|  
  
## <a name="BKMK_Install"></a>2.1 安装角色和功能  
为每个服务器，你想要加入群集时，计划安装远程访问角色。 此外计划安装网络负载平衡 (NLB) 功能，如果你想要流量进行负载均衡到使用 Windows NLB 群集。 有关详细信息请参阅[网络负载平衡](https://technet.microsoft.com/windows-server-docs/networking/technologies/network-load-balancing)。  
  
## <a name="BKMK_Config"></a>2.2 配置服务器设置  
对于每个服务器将添加到群集，请规划 IP 地址和域设置。 请注意以下事项：  
  
1.  群集中的服务器必须全部属于同一个域。  
  
2.  群集中的服务器必须位于同一子网。  
  
3.  在群集中的每个服务器应在用于 DirectAccess 部署中具有相同数量的网络适配器。  
  
负载均衡使用 Windows NLB 群集时将应用以下 Windows NLB 设置：  
  
1.  操作模式单播。 这可以更改为使用 NLB 管理器的多播。 无法在远程访问管理控制台中修改此设置。  
  
2.  负载的权重身份定义为相等的其中所有群集服务器都具有相等的负载。  
  
3.  筛选模式通信将负载平衡跨多个主机。  
  
4.  定义关联-单个相关性。  
  
5.  同时协议  

