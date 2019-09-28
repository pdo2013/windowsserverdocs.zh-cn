---
title: 步骤2规划 RADIUS 服务器部署
description: 本主题是指南使用 Windows Server 2016 中的 "使用 OTP 身份验证部署远程访问" 指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2d6ad863-02a5-49b0-9aff-d189e78b2b80
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a991b312a0938a3809acd2b94c00aa678f5b41da
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404399"
---
# <a name="step-2-plan-the-radius-server-deployment"></a>步骤2规划 RADIUS 服务器部署

>适用于：Windows Server（半年频道）、Windows Server 2016

部署单个远程访问服务器后，请规划一次性密码（OTP）身份验证服务器。  
  
|任务|描述|  
|----|--------|  
|2.1 规划 RADIUS 服务器|对于 OTP 身份验证服务器，Windows Server 2016 和 Windows Server 2012 中的远程访问支持支持支持密码身份验证协议（PAP）的启用 RADIUS 的 OTP 服务器。|  
  
## <a name="BKMK_1.1"></a>2.1 规划 RADIUS 服务器  
在规划用于 OTP 身份验证的 RADIUS 服务器时，请注意以下事项：  
  
-   对于大多数类型的 OTP 部署，你必须将远程访问服务器配置为 RADIUS 代理。 有关详细信息，请参阅 OTP 供应商文档。  
  
-   对于所有 OTP 部署，必须将 Active Directory 用户与 RADIUS 服务器同步。  
  
-   RADIUS 服务器不需要是域成员。  
  
-   部署 RADIUS 服务器时，需要为 RADIUS 流量配置共享机密和端口号。 记下这些详细信息;当你配置远程访问服务器时，它们是必需的。  
  
你可以在 [Test 实验室指南中查看使用 RSA SecurID 服务器设置 OTP 身份验证的示例测试实验室指南：演示具有 OTP 身份验证和 RSA SecurID @ no__t 的 DirectAccess。  
  
  
  


