---
title: 规划带有 OTP 身份验证的远程访问
description: 本主题是指南部署带有 OTP 身份验证，Windows Server 2016 中的远程访问的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 762bc463-eead-46ac-8b90-32355743c27c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fafc41ef5dbd2cd7f98fa2552426a97622ec8634
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863218"
---
# <a name="plan-remote-access-with-otp-authentication"></a>规划带有 OTP 身份验证的远程访问

>适用于：Windows 服务器 （半年频道），Windows Server 2016

 Windows Server 2016 和 Windows Server 2012 将 DirectAccess 以及路由和远程访问服务 (RRAS) VPN 合并到单个远程访问角色。 本概述介绍为了部署单个 Windows Server 2016 或 Windows Server 2012 远程访问多站点部署所需的配置步骤。  
  
  
-  第 1 步：[部署单个 DirectAccess 服务器使用高级设置](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/single-server-advanced/deploy-a-single-directaccess-server-with-advanced-settings)。 此步骤包括规划部署一台服务器所需的基础结构。 它包括规划网络和服务器设置、 证书要求、 DNS 设置、 网络位置服务器部署、 DirectAccess 管理服务器、 Active Directory 设置和组策略对象 (Gpo)。  
  
-   [步骤 2：规划 RADIUS 服务器部署](Step-2-Plan-the-RADIUS-Server-Deployment.md)  
  
-   [步骤 3：计划 OTP 证书部署](Step-3-Plan-OTP-Certificate-Deployment.md)  
  
-   [步骤 4：规划远程访问服务器上的 OTP](Step-4-Plan-for-OTP-on-the-Remote-Access-Server.md)  
  
完成这些规划步骤后，请参阅[配置带有 OTP 身份验证的远程访问](https://technet.microsoft.com/windows-server-docs/networking/remote-access/ras/otp/configure/configure-ra-with-otp-authentication)。 作为一个概念证明在实验室环境中配置多站点部署的信息，请参阅[测试实验室指南：演示带有 OTP 身份验证和 RSA SecurID 的 DirectAccess](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/tlg-otp-securid/test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid)。  
  


