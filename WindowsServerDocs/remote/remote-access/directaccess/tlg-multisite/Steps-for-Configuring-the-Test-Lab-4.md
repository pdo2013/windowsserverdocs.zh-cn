---
title: 配置测试实验室的步骤
description: 本主题是测试实验室指南-演示适用于 Windows Server 2016 的 DirectAccess 多站点部署的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc7205b4-a822-4038-ab67-ec0a870737f2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dd8b8864dff98e51bf55aad9307523df4a0c30bf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404692"
---
# <a name="steps-for-configuring-the-test-lab"></a>配置测试实验室的步骤

>适用于：Windows Server（半年频道）、Windows Server 2016

以下步骤介绍如何配置远程访问基础结构、配置远程访问服务器和客户端以及通过 Internet 和 Homenet 子网测试 DirectAccess 连接。  
  
在此测试实验室指南中，你将通过执行以下步骤来生成多站点远程访问部署：  
  
-   [步骤 1：完成基本配置 @ no__t-0。 完成 [Test 实验室指南中的所有步骤：演示具有混合 IPv4 和 IPv6 @ no__t 的 DirectAccess 单一服务器设置。  
  
-   [步骤 2：安装并配置 ROUTER1 @ no__t-0。 ROUTER1 提供了公司网络和2公司网络子网之间的路由和转发功能。  
  
-   [步骤 3：安装并配置 CLIENT2 @ no__t-0。 CLIENT2 是一台 Windows 7 客户端计算机，用于演示 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 远程访问部署的向后兼容性。  
  
-   [步骤 4：配置 APP1 @ no__t-0。 将 APP1 配置为 ROUTER1 作为默认网关，并将 2-DC1 配置为备用 DNS 服务器。  
  
-   [步骤 5：配置 DC1 @ no__t。 使用其他 Active Directory 站点和其他安全组为 DC1 配置 Windows 7 客户端计算机。  
  
-   [步骤 6：安装并配置 2-DC1 @ no__t-0。 在多站点部署中，你有两个或多个域和站点。 2-DC1 为 corp2.corp.contoso.com 域提供域控制器和 DNS 服务。  
  
-   [步骤 7：安装并配置 APP1 @ no__t-0。 2-APP1 是2公司网络中的 web 和文件服务器。  
  
-   [步骤 8：配置 INET1 @ no__t-0。 本测试实验室指南中的 INET1 模拟 Internet。 必须配置可解析为 EDGE1 的公共 IP 地址的 DNS 条目。  
  
-   [步骤 9：配置 EDGE1 @ no__t-0。 在 EDGE1 上配置2公司网络 DNS 服务器和路由。  
  
-   [步骤 10：安装并配置 EDGE1 @ no__t-0。 多站点部署中需要两个远程访问服务器。 2-EDGE1 为第二个域提供远程访问服务。  
  
-   [步骤 11：配置多站点部署 @ no__t。 配置两个远程访问服务器后，可以配置多站点部署。  
  
-   [步骤 12：测试 DirectAccess 连接性 @ no__t。 通过 EDGE1 和 2-EDGE1 从 Internet 子网测试两个客户端计算机的 DirectAccess 连接。  
  
-   [步骤 13：从 NAT 设备后面测试 DirectAccess 连接 @ no__t-0。 从 NAT 设备后面测试 DirectAccess 连接。  
  
-   [步骤 14：快照配置 @ no__t。 完成测试实验室后，拍摄工作远程访问多站点部署的快照，以便稍后可以返回到测试其他方案。  
  


