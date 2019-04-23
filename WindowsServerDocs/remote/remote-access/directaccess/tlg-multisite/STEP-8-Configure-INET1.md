---
title: 步骤 8 配置 INET1
description: 本主题是测试实验室指南的一部分-演示 DirectAccess 多站点部署的 Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 693acb5c-dffc-4484-8286-163bb67724c9
ms.author: coreyp
author: coreyp-at-msft
ms.openlocfilehash: 707591604a1d030b3abba9395081d2c2e4b7fb1c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838808"
---
# <a name="step-8-configure-inet1"></a>步骤 8:配置 INET1

>适用于：Windows 服务器 （半年频道），Windows Server 2016

若要使客户端计算机能够通过 Internet 连接到远程访问服务器，必须上 INET1 配置 2 EDGE1 DNS 条目。  
  
### <a name="to-create-the-2-edge1-dns-entry"></a>若要创建 2 EDGE1 DNS 条目  
  
1.  上**启动**屏幕上，键入**dnsmgmt.msc**，然后按 ENTER。  
  
2.  在控制台树中，打开**正向查找区域**，单击**contoso.com**，然后右键单击**contoso.com**，然后单击**新建主机 （A 或 AAAA）**.  
  
3.  在中**名称**，类型**2 EDGE1**。 在中**IP 地址**，类型**131.107.0.20**。 依次单击“添加主机”、“确定”，然后单击“完成”。  
  


