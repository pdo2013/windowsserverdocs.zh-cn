---
title: 配置测试实验室的步骤
description: 本主题是测试实验室指南的一部分-演示带有 OTP 身份验证的 DirectAccess 和用于 Windows Server 2016 的 RSA SecurID
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0a40183c-afd1-43ca-b306-05745640a37d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ef5ce37983b8565fab8287eeaae7423be0c269f6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404727"
---
# <a name="steps-for-configuring-the-test-lab"></a>配置测试实验室的步骤

>适用于：Windows Server（半年频道）、Windows Server 2016

以下步骤介绍了如何配置远程访问基础结构、配置远程访问服务器和客户端，以及如何通过 Homenet 和 Internet 子网测试 DirectAccess 连接。  
  
在此测试实验室指南中，你将通过执行以下步骤来构建使用 OTP 环境的远程访问：  
  
-   [步骤 1：完成 DirectAccess 配置 @ no__t。 完成 [Test 实验室指南中的所有步骤：演示具有混合 IPv4 和 IPv6 @ no__t 的 DirectAccess 单一服务器设置。  
  
-   [步骤 2：配置 APP1 @ no__t-0。 使用 OTP 证书模板配置 APP1 以供 EDGE1 使用。  
  
-   [步骤 3：配置 DC1 @ no__t。 验证在 DC1 上定义的用户主体名称。  
  
-   [步骤 7：安装并配置 RSA @ no__t-0。 安装和配置 RSA、RADIUS 和 OTP 服务器，并为 OTP 配置 EDGE1。  
  
-   [步骤 11：验证 EDGE1 @ no__t 上的 OTP 运行状况。 请确保远程访问服务器上的 OTP 状态为 "正常"。  
  
-   [步骤 8：从 Homenet 子网 @ no__t 测试 DirectAccess 连接。 从 NAT 设备后面测试 DirectAccess OTP 功能。  
  
-   [步骤 10：从 Internet @ no__t 测试 DirectAccess 连接。 测试来自 Internet 的 DirectAccess 客户端连接。  
  
-   [步骤 12：快照配置 @ no__t。 完成测试实验室后，使用 OTP 配置拍摄工作 DirectAccess 的快照，以便稍后可以返回到测试其他方案。  
  


