---
title: DirectAccess 群集 NLB 测试实验室方案概述
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
ms.assetid: cd1e9efd-19e9-49e7-8432-881f661c9792
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 118a07b761979d3c32a8f7cf4f149aed56a1a893
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863578"
---
# <a name="overview-of-the-directaccess-cluster-nlb-test-lab-scenario"></a>DirectAccess 群集 NLB 测试实验室方案概述

>适用于：Windows 服务器 （半年频道），Windows Server 2016

在此测试实验室方案中，DirectAccess 部署有：  
  
-   **DC1**-的服务器配置为域控制器、 域名系统 (DNS) 服务器和动态主机配置协议 (DHCP) 服务器。  
  
-   **EDGE1**配置为远程访问服务器群集中的第一个远程访问服务器的内部网络上的服务器。 此服务器有两个网络适配器;一个连接到内部网络和其他连接到外部网络。  
  
-   **EDGE2**配置为远程访问服务器群集中的第二个远程访问服务器的内部网络上的服务器。 此服务器有两个网络适配器;一个连接到内部网络和其他连接到外部网络。  
  
-   **APP1**将配置为 web 服务和文件服务器，并为企业根证书颁发机构 (CA) 的内部网络上的服务器  
  
-   **APP2**-配置为 IPv4 唯一的 web 服务和文件服务器在内部网络计算机。 使用此计算机来突出显示 nat64/dns64 功能。  
  
-   **INET1**配置为 Internet DNS 和 DHCP 服务器的服务器。  
  
-   **NAT1**-客户端计算机配置为使用 Internet 连接共享的网络地址转换器 (NAT) 设备。  
  
-   **CLIENT1**配置为 DirectAccess 客户端将用于内部网络中，模拟的 Internet 和家庭网络之间切换时测试 DirectAccess 连接的客户端计算机。  
  
测试实验室包含模拟以下的三个子网：  
  
-   名为 Homenet (192.168.137.0/24) 的家庭网络连接到 Internet 的 nat 后面。  
  
-   由 Internet 子网 (131.107.0.0/24) 表示的外部网络。  
  
-   名为 Corpnet 的内部网络 (10.0.0.0/24; 2001:db8:1:: / 64) 从 Internet 分隔远程访问服务器。  
  
每个子网上的计算机使用物理或虚拟集线器或交换机，如下图中所示连接。  
  
![测试实验室概述](../../../media/Overview-of-the-Test-Lab-Scenario_5/TLG_DA_Cluster.png)  
  


