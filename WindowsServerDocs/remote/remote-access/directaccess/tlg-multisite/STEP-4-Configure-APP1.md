---
title: 步骤 4 配置 APP1
description: 本主题是测试实验室指南的一部分-演示 DirectAccess 多站点部署的 Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7000e80f-31b1-43c5-b51e-1469d26909e5
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 208839b827965d5fdbef4927f25a2477e117999b
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283193"
---
# <a name="step-4-configure-app1"></a>步骤 4 配置 APP1

>适用于：Windows 服务器 （半年频道），Windows Server 2016

配置静态 IPv6 寻址和网关设置以启用 APP1 访问 2 Corpnet 子网。  
  
- 若要配置默认网关和 DNS 服务器。 多站点配置使用路由器 1 计算机作为默认网关。 在 APP1 上配置默认网关。  
  
## <a name="to-configure-the-default-gateway-and-dns-server"></a>若要配置默认网关和 DNS 服务器  
  
1.  在服务器管理器控制台中，单击**本地服务器**，然后在**属性**区域中，为下的一步**有线以太网连接**，单击的链接。  
  
2.  在中**网络连接**窗口中，右键单击**有线以太网连接**，然后单击**属性**。  
  
3.  上**有线以太网连接属性**对话框中，单击**Internet 协议版本 4 (TCP/IPv4)** ，然后单击**属性**。  
  
4.  在中**默认网关**，类型**10.0.0.254**，然后在**备用 DNS 服务器**，类型**10.2.0.1**，然后单击**确定**.  
  
5.  上**有线以太网连接属性**对话框中，单击**Internet 协议版本 6 (Tcp/ipv6)** ，然后单击**属性**。  
  
6.  在中**默认网关**，类型**2001:db8:1::fe**。 在中**备用 DNS 服务器**，类型**2001:db8:2::1**，然后单击**确定**。  
  
7.  上**有线以太网连接属性**对话框中，单击**关闭**，然后关闭**网络连接**窗口。  
  


