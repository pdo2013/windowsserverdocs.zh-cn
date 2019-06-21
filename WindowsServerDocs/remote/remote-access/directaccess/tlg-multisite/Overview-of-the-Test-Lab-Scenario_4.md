---
title: 测试实验室方案概述
description: 本主题是测试实验室指南的一部分-演示 DirectAccess 多站点部署的 Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9afeced4-1a9b-4cb3-9fc4-d7e44c675755
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b067d5f247f5dc13ea294d83a76f267c5a57ffe7
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283299"
---
# <a name="overview-of-the-test-lab-scenario"></a>测试实验室方案概述

>适用于：Windows 服务器 （半年频道），Windows Server 2016

在此测试实验室方案中，DirectAccess 部署有：  
  
-   **DC1**配置为域控制器的服务器、 DNS 服务器和为 corp.contoso.com 域的 DHCP 服务器。  
  
-   **2-DC1**配置为域控制器和 corp2.corp.contoso.com 域的 DNS 服务器的服务器。  
  
-   **EDGE1 和 2 EDGE1**-内部网络上配置为远程访问服务器的两个服务器。 每个服务器有两个网络适配器;一个连接到内部网络和其他连接到外部网络。  
  
-   **APP1 和 2-APP1**-配置为 web 和文件服务器的内部网络上的两个服务器。  
  
-   **APP2**-配置为 IPv4 唯一的 web 服务和文件服务器在内部网络计算机。 使用此计算机来突出显示 nat64/dns64 功能。  
  
-   **路由器 1**-的服务器配置为提供两个公司内部网络之间的路由。  
  
-   **INET1**配置为 Internet DNS 和 DHCP 服务器的服务器。  
  
-   **NAT1**-客户端计算机配置为使用 Internet 连接共享的网络地址转换器 (NAT) 设备。  
  
-   **CLIENT1 和 CLIENT2**-配置为 DirectAccess 客户端将用于内部网络中，模拟的 Internet 和家庭网络之间切换时测试 DirectAccess 连接的两个客户端计算机。 **CLIENT2**是 Windows 7&reg;客户端。  
  
测试实验室包含四个模拟以下的子网：  
  
-   名为 Homenet (192.168.137.0/24) 的家庭网络连接到 Internet 的 nat 后面。  
  
-   由 Internet 子网 (131.107.0.0/24) 表示的外部网络。  
  
-   名为 Corpnet 的内部网络 (10.0.0.0/24; 2001:db8:1:: / 64) 从 Internet 分隔 EDGE1 远程访问服务器。  
  
-   内部网络名为 2 Corpnet1 (10.2.0.0/24; 2001:db8:2:: / 64) 从 Internet 分隔 2 EDGE1 远程访问服务器。  
  
每个子网上的计算机使用物理或虚拟集线器或交换机，如下图中所示连接。  
  
![测试实验室概述](../../../media/Overview-of-the-Test-Lab-Scenario_4/TLG_DA_Multisite.png)  
  


