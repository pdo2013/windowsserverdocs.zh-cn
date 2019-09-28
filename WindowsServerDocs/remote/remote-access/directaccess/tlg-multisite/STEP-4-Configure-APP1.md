---
title: 步骤4配置 APP1
description: 本主题是测试实验室指南-演示适用于 Windows Server 2016 的 DirectAccess 多站点部署的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7000e80f-31b1-43c5-b51e-1469d26909e5
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dc4715fcec778d1fa63ff84e801961572b9cd589
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404780"
---
# <a name="step-4-configure-app1"></a>步骤4配置 APP1

>适用于：Windows Server（半年频道）、Windows Server 2016

配置静态 IPv6 寻址和网关设置，以启用对2公司网络子网的 APP1 访问。  
  
- 配置默认网关和 DNS 服务器。 多站点配置使用 ROUTER1 计算机作为默认网关。 在 APP1 上配置默认网关。  
  
## <a name="to-configure-the-default-gateway-and-dns-server"></a>配置默认网关和 DNS 服务器  
  
1.  在服务器管理器控制台中，单击 "**本地服务器**"，然后在 "**有线以太网连接**" 旁边的 "**属性**" 区域中，单击链接。  
  
2.  在 "**网络连接**" 窗口中，右键单击 "**有线以太网连接**"，然后单击 "**属性**"。  
  
3.  在 "**有线以太网连接属性**" 对话框中，单击 " **Internet 协议版本4（TCP/IPv4）** "，然后单击 "**属性**"。  
  
4.  在 "**默认网关**" 中，键入**10.0.0.254**，在 "**备用 DNS 服务器**" 中键入**10.2.0.1**，然后单击 **"确定"** 。  
  
5.  在 "**有线以太网连接属性**" 对话框中，单击 " **Internet 协议版本6（TCP/IPv6）** "，然后单击 "**属性**"。  
  
6.  在 "**默认网关**" 中，键入 " **2001： db8：1：： fe**"。 在 "**备用 DNS 服务器**" 中，键入 " **2001： db8：2：： 1**"，然后单击 **"确定"** 。  
  
7.  在 "**有线以太网连接属性**" 对话框中，单击 "**关闭**"，然后关闭 "**网络连接**" 窗口。  
  


