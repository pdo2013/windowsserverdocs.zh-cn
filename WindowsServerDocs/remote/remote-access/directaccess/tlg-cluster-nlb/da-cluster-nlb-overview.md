---
title: DirectAccess 群集 NLB 测试实验室方案概述
description: 本主题是测试实验室指南的一部分-使用 windows Server 2016 的 Windows NLB 在群集中演示 DirectAccess
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cd1e9efd-19e9-49e7-8432-881f661c9792
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 487b85713d415cdda9ee40548091abdb5fe7fc2e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404841"
---
# <a name="overview-of-the-directaccess-cluster-nlb-test-lab-scenario"></a>DirectAccess 群集 NLB 测试实验室方案概述

>适用于：Windows Server（半年频道）、Windows Server 2016

在此测试实验室方案中，DirectAccess 是通过以下方式部署的：  
  
-   **DC1**-配置为域控制器、域名系统（DNS）服务器和动态主机配置协议（DHCP）服务器的服务器。  
  
-   **EDGE1**-内部网络上配置为远程访问服务器群集中的第一个远程访问服务器的服务器。 此服务器有两个网络适配器;一个连接到内部网络，另一个连接到外部网络。  
  
-   **EDGE2**-内部网络上配置为远程访问服务器群集中的第二个远程访问服务器的服务器。 此服务器有两个网络适配器;一个连接到内部网络，另一个连接到外部网络。  
  
-   **APP1**-配置为 web 和文件服务器的内部网络上的服务器，以及作为企业根证书颁发机构（CA）的服务器  
  
-   **APP2**-内部网络上的一台计算机，该计算机配置为仅限 IPv4 的 web 和文件服务器。 此计算机用于突出显示 NAT64/DNS64 功能。  
  
-   **INET1**-配置为 Internet DNS 和 DHCP 服务器的服务器。  
  
-   **NAT1**-使用 Internet 连接共享配置为网络地址转换器（NAT）设备的客户端计算机。  
  
-   **CLIENT1**-配置为 directaccess 客户端的客户端计算机，在内部网络、模拟 Internet 和家庭网络之间移动时，将使用该客户端来测试 directaccess 连接。  
  
测试实验室包含三个子网，用于模拟以下内容：  
  
-   一个名为 Homenet （192.168.137.0/24）的家庭网络，该网络通过 NAT 连接到 Internet。  
  
-   由 Internet 子网（131.107.0.0/24）表示的外部网络。  
  
-   远程访问服务器与 Internet 分离的名为公司网络（10.0.0.0/24; 2001： db8：1：：/64）的内部网络。  
  
每个子网上的计算机使用物理或虚拟集线器或交换机连接，如下图所示。  
  
![测试实验室概述](../../../media/Overview-of-the-Test-Lab-Scenario_5/TLG_DA_Cluster.png)  
  


