---
title: 步骤 2 规划 RADIUS 服务器部署
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
ms.assetid: 2d6ad863-02a5-49b0-9aff-d189e78b2b80
ms.author: pashort
author: shortpatti
ms.openlocfilehash: faf3f0b7c691edfb2c41e7b568e0791a3cad76b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859208"
---
# <a name="step-2-plan-the-radius-server-deployment"></a>步骤 2 规划 RADIUS 服务器部署

>适用于：Windows 服务器 （半年频道），Windows Server 2016

部署单一远程访问服务器之后, 计划的一次性密码 (OTP) 身份验证服务器。  
  
|任务|描述|  
|----|--------|  
|2.1 规划 RADIUS 服务器|对于 OTP 身份验证服务器，请在 Windows Server 2016 和 Windows Server 2012 中的远程访问支持任何支持密码身份验证协议 (PAP) 的启用 RADIUS 的 OTP 服务器。|  
  
## <a name="BKMK_1.1"></a>2.1 规划 RADIUS 服务器  
规划 RADIUS 服务器进行 OTP 身份验证时，请注意以下：  
  
-   对于大多数类型的 OTP 部署，必须为 RADIUS 代理配置远程访问服务器。 有关详细信息，请参阅 OTP 供应商文档。  
  
-   对于所有 OTP 部署，必须与 RADIUS 服务器来同步 Active Directory 用户。  
  
-   RADIUS 服务器不需要是域成员。  
  
-   当你部署 RADIUS 服务器时，配置共享的机密和对 RADIUS 流量的端口号。 请记下的这些详细信息;配置远程访问服务器时，它们是必需的。  
  
您可以查看设置了使用中的 RSA SecurID 服务器进行 OTP 身份验证的示例测试实验室指南[测试实验室指南：演示带有 OTP 身份验证和 RSA SecurID 的 DirectAccess](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/tlg-otp-securid/test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid)。  
  
  
  


