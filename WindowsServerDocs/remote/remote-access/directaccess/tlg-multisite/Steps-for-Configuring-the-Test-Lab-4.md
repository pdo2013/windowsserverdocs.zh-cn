---
title: 配置测试实验室的步骤
description: 本主题是测试实验室指南的一部分-演示 DirectAccess 多站点部署的 Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc7205b4-a822-4038-ab67-ec0a870737f2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a3d01dd8002e28fb127ac6b1b4cea25c58953521
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281393"
---
# <a name="steps-for-configuring-the-test-lab"></a>配置测试实验室的步骤

>适用于：Windows 服务器 （半年频道），Windows Server 2016

以下步骤介绍如何配置远程访问基础结构、 配置远程访问服务器和客户端和测试从 Internet 和 Homenet 子网的 DirectAccess 连接。  
  
在本测试实验室指南中将生成多站点远程访问部署中，通过执行以下步骤：  
  
-   [步骤 1：完成基础配置](assetId:///9eb4a9ba-9118-4ea3-8963-e643ec81c3ed)。 完成中的所有步骤[测试实验室指南：演示混合使用 IPv4 和 IPv6 的 DirectAccess 单服务器安装](https://go.microsoft.com/fwlink/p/?LinkId=237004)。  
  
-   [步骤 2：安装和配置路由器 1](assetId:///e4b1a298-d5b0-410e-970b-c5358a9378f9)。 路由器 1 提供路由和转发公司网络和 2 Corpnet 子网之间的功能。  
  
-   [步骤 3：安装和配置 CLIENT2](assetId:///6cbee1b5-f6f6-443f-8fa9-31cc5c05a0ee)。 CLIENT2 是用于演示的 Windows 7 客户端计算机向后兼容性的 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 远程访问部署。  
  
-   [步骤 4：配置 APP1](assetId:///a0ee655e-c01e-4bf3-a7b3-064e9614f810)。 配置 APP1 与路由器 1 作为默认网关和 2-DC1 作为备用 DNS 服务器。  
  
-   [步骤 5：配置 DC1](assetId:///205ca795-93ce-4e53-aa6b-b44c87f0e14a)。 使用其他 Active Directory 站点和 Windows 7 客户端计算机的其他安全组配置 DC1。  
  
-   [步骤 6：安装和配置 2 DC1](assetId:///16752f61-edbf-4ff4-9d7a-e2077b66a127)。 在多站点部署中，有两个或多个域和站点。 2-DC1 提供域控制器和 corp2.corp.contoso.com 域的 DNS 服务。  
  
-   [步骤 7：安装和配置 2-APP1](assetId:///7d04b54e-590a-4d33-9766-415789859f29)。 2-APP1 是 2 Corpnet 网络中的 web 服务和文件服务器。  
  
-   [步骤 8：配置 INET1](assetId:///8ecc0b63-8626-4939-8d26-3d51d051d231)。 INET1 模拟本测试实验室指南中的 Internet。 必须配置 DNS 条目解析为 2 EDGE1 的公共 IP 地址。  
  
-   [步骤 9：配置 EDGE1](assetId:///562744dc-30f6-42fa-bd5f-60a013b2179e)。 配置 2 公司网络 DNS 服务器和 EDGE1 上的路由。  
  
-   [步骤 10：安装和配置 2 EDGE1](assetId:///1938c4f3-ca96-475d-9f2e-6bea3b7a4130)。 在多站点部署中需要两个远程访问服务器。 2 EDGE1 提供第二个域的远程访问服务。  
  
-   [步骤 11：配置多站点部署](assetId:///537e4b68-043f-49c9-94d8-15ce8c4b18e2)。 在配置两台远程访问服务器之后, 您可以配置多站点部署。  
  
-   [步骤 12：测试 DirectAccess 连接](assetId:///aa293b5d-4b6f-4004-95f3-0ab54804b15c)。 测试来自客户端计算机通过 EDGE1 和 2 EDGE1 Internet 子网中的 DirectAccess 连接。  
  
-   [步骤 13：测试从 NAT 设备后面的 DirectAccess 连接](assetId:///41f8195b-00a1-4991-9db8-3703514dbe0c)。 测试从 NAT 设备后面的 DirectAccess 连接。  
  
-   [步骤 14：拍摄配置快照](assetId:///7b56d5c9-c334-463e-9e29-d652ca110d84)。 完成测试实验后，拍摄了远程访问多站点部署中的快照，以便可以返回到更高版本以测试其他方案。  
  


