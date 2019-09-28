---
title: 配置 DirectAccess 群集 NLB 测试实验室的步骤
description: 本主题是测试实验室指南的一部分-使用 windows Server 2016 的 Windows NLB 在群集中演示 DirectAccess
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e508d3ee-ffa6-463f-a3dd-9e35e745c005
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 108c63298ad3382f5ece790258f2d278bb03b78b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388401"
---
# <a name="steps-for-configuring-the-directaccess-cluster-nlb-test-lab"></a>配置 DirectAccess 群集 NLB 测试实验室的步骤

>适用于：Windows Server（半年频道）、Windows Server 2016

以下步骤介绍了如何配置远程访问基础结构、配置远程访问服务器和客户端，以及如何通过 Internet 和 Homenet 子网测试 DirectAccess 连接。  
  
在本测试实验室指南中，你将通过执行以下步骤来构建网络负载平衡（NLB）启用的远程访问群集：  
  
-   [步骤1：完成 DirectAccess 配置](STEP-1-Complete-the-DirectAccess-Configuration.md)。 完成 [Test 实验室指南中的所有步骤：演示具有混合 IPv4 和 IPv6 @ no__t 的 DirectAccess 单一服务器设置。  
  
-   [步骤 2：配置 EDGE1 @ no__t-0。 在 EDGE1 上配置远程访问角色以实现负载平衡。  
  
-   [步骤 3：安装并配置 EDGE2 @ no__t-0。 EDGE2 充当远程访问群集中的第二个远程访问服务器。  
  
-   [步骤 4：创建网络负载平衡远程访问群集 @ no__t-0-EDGE1 配置为远程访问群集中的第一个服务器。 EDGE2 加入群集，并为群集配置 NLB。  
  
-   [步骤 5：通过 Internet 和群集 @ no__t 测试 DirectAccess 连接。 NLB 和群集配置完成后，可以通过负载平衡群集测试 DirectAccess 客户端连接。  
  
-   [步骤 6：从 NAT 设备后面测试 DirectAccess 客户端连接 @ no__t-0。 将客户端计算机移动到 NAT 设备后面，以模拟从主路由器后面测试 DirectAccess 客户端连接。  
  
-   [步骤 7：返回公司网络 @ no__t 时测试连接性。 确保客户端计算机在返回到公司资源时仍可访问公司资源。  
  
-   [步骤 8：快照配置 @ no__t。 完成测试实验室后，拍摄工作远程访问 NLB 群集的快照，以便稍后可将其返回到测试其他方案。  
  


