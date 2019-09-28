---
title: 测试实验室方案概述
description: 本主题是测试实验室指南-演示适用于 Windows Server 2016 的 DirectAccess 多站点部署的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9afeced4-1a9b-4cb3-9fc4-d7e44c675755
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bcee25c4a13afc2b41d6b1a9a43a1489054ef43d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388406"
---
# <a name="overview-of-the-test-lab-scenario"></a>测试实验室方案概述

>适用于：Windows Server（半年频道）、Windows Server 2016

在此测试实验室方案中，DirectAccess 是通过以下方式部署的：  
  
-   **DC1**-配置为域控制器、DNS 服务器以及 corp.contoso.com 域的 DHCP 服务器的服务器。  
  
-   **2-DC1**-配置为域控制器和 corp2.corp.contoso.com 域的 DNS 服务器的服务器。  
  
-   **EDGE1 和 EDGE1**-配置为远程访问服务器的内部网络上的两台服务器。 每个服务器都有两个网络适配器;一个连接到内部网络，另一个连接到外部网络。  
  
-   **APP1 和 APP1**-在内部网络上配置为 web 和文件服务器的两个服务器。  
  
-   **APP2**-内部网络上的一台计算机，该计算机配置为仅限 IPv4 的 web 和文件服务器。 此计算机用于突出显示 NAT64/DNS64 功能。  
  
-   **ROUTER1**-配置为在两个企业内部网络之间提供路由的服务器。  
  
-   **INET1**-配置为 Internet DNS 和 DHCP 服务器的服务器。  
  
-   **NAT1**-使用 Internet 连接共享配置为网络地址转换器（NAT）设备的客户端计算机。  
  
-   **CLIENT1 和 CLIENT2**-在内部网络、模拟 Internet 和家庭网络之间移动时，配置为 directaccess 客户端的两个客户端计算机将用于测试 directaccess 连接。 **CLIENT2**是 Windows 7 @ no__t-1 客户端。  
  
测试实验室由四个子网组成，其中模拟以下内容：  
  
-   一个名为 Homenet （192.168.137.0/24）的家庭网络，该网络通过 NAT 连接到 Internet。  
  
-   由 Internet 子网（131.107.0.0/24）表示的外部网络。  
  
-   一种名为公司网络（10.0.0.0/24; 2001： db8：1：：/64）由 EDGE1 远程访问服务器与 Internet 分离的内部网络。  
  
-   一个名为 Corpnet1 （10.2.0.0/24; 2001： db8：2：：/64）的内部网络由 2-EDGE1 远程访问服务器与 Internet 分离。  
  
每个子网上的计算机使用物理或虚拟集线器或交换机连接，如下图所示。  
  
![测试实验室概述](../../../media/Overview-of-the-Test-Lab-Scenario_4/TLG_DA_Multisite.png)  
  


