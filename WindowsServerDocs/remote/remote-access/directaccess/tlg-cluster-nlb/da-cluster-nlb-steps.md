---
title: 配置 DirectAccess 群集 NLB 测试实验室的步骤
description: 本主题是一部分的测试实验室指南-使用 Windows Server 2016 的 Windows NLB 的群集中演示 DirectAccess
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e508d3ee-ffa6-463f-a3dd-9e35e745c005
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3a243debf79d2c9d12de511153b74f23c5a44cce
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880738"
---
# <a name="steps-for-configuring-the-directaccess-cluster-nlb-test-lab"></a>配置 DirectAccess 群集 NLB 测试实验室的步骤

>适用于：Windows 服务器 （半年频道），Windows Server 2016

以下步骤介绍如何配置远程访问基础结构、 配置远程访问服务器和客户端，并测试从 Internet 和 Homenet 子网的 DirectAccess 连接。  
  
在此测试实验室指南将生成网络负载平衡 (NLB) 启用远程访问群集，请执行以下步骤：  
  
-   [第 1 步完成 DirectAccess 配置](STEP-1-Complete-the-DirectAccess-Configuration.md)。 完成中的所有步骤[测试实验室指南：演示混合使用 IPv4 和 IPv6 的 DirectAccess 单服务器安装](https://go.microsoft.com/fwlink/p/?LinkId=237004)。  
  
-   [步骤 2:配置 EDGE1](STEP-2-Configure-EDGE1.md)。 为负载平衡 EDGE1 上配置远程访问角色。  
  
-   [步骤 3:安装和配置 EDGE2](STEP-3-Install-and-Configure-EDGE2.md)。 EDGE2 充当远程访问群集中的第二个远程访问服务器。  
  
-   [步骤 4:创建网络负载平衡远程访问群集](STEP-4-Create-the-Network-Load-Balanced-Remote-Access-Cluster.md)-EDGE1 配置为远程访问群集中的第一个服务器。 EDGE2 加入到群集和 NLB 配置群集。  
  
-   [步骤 5:测试从 Internet 以及群集通过 DirectAccess 连接](STEP-5-Test-DirectAccess-Connectivity-from-the-Internet-and-Through-the-Cluster.md)。 NLB 和群集配置完成后可以测试通过负载平衡群集的 DirectAccess 客户端连接。  
  
-   [步骤 6:测试从 NAT 设备后面的 DirectAccess 客户端连接](STEP-6-Test-DirectAccess-Client-Connectivity-from-Behind-a-NAT-Device.md)。 移动客户端计算机在 NAT 设备，若要模拟从家用路由器后面测试 DirectAccess 客户端连接后面。  
  
-   [步骤 7:返回到公司网络时测试连接](STEP-7-Test-Connectivity-When-Returning-to-the-Corpnet.md)。 请确保客户端计算机仍然可以访问公司资源时返回到公司网络。  
  
-   [步骤 8:拍摄配置快照](da-cluster-nlb-s8-snapshot.md)。 完成测试实验后，拍摄了远程访问 NLB 群集的快照，以便可以返回到更高版本以测试其他方案。  
  


