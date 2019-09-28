---
title: 步骤8配置 INET1
description: 本主题是测试实验室指南-演示适用于 Windows Server 2016 的 DirectAccess 多站点部署的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 693acb5c-dffc-4484-8286-163bb67724c9
ms.author: coreyp
author: coreyp-at-msft
ms.openlocfilehash: 54674adc33f45a58f2515d07fed4c8a070ded5a1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404705"
---
# <a name="step-8-configure-inet1"></a>步骤8：配置 INET1

>适用于：Windows Server（半年频道）、Windows Server 2016

若要使客户端计算机能够通过 Internet 连接到远程访问服务器，必须在 INET1 上为 EDGE1 配置 DNS 条目。  
  
### <a name="to-create-the-2-edge1-dns-entry"></a>创建 EDGE1 DNS 条目  
  
1.  在 "**开始**" 屏幕上，键入 "**dnsmgmt.msc**"，然后按 enter。  
  
2.  在控制台树中，打开 "**正向查找区域**"，单击 " **contoso.com**"，右键单击 " **contoso.com**"，然后单击 "**新建主机（A 或 AAAA）** "。  
  
3.  在 "**名称**" 中，键入**2-EDGE1**。 在 " **IP 地址**" 中，键入**131.107.0.20**。 依次单击“添加主机”、“确定”，然后单击“完成”。  
  


