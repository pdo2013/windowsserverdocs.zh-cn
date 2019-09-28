---
title: 步骤4在远程访问服务器上计划 OTP
description: 本主题是指南使用 Windows Server 2016 中的 "使用 OTP 身份验证部署远程访问" 指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b97b2fd-767a-45c1-a64e-5b3edd0c8a47
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cc833ea2ae5d24754a445d6c1252f21a59cc6f13
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404388"
---
# <a name="step-4-plan-for-otp-on-the-remote-access-server"></a>步骤4在远程访问服务器上计划 OTP

>适用于：Windows Server（半年频道）、Windows Server 2016

规划一次性密码（OTP） RADIUS 服务器和证书设置之后，规划远程访问 OTP 部署的最后一步是在远程访问服务器上规划客户端 OTP 设置。  
  
|任务|描述|  
|----|--------|  
|[4.1 OTP 客户端豁免计划](#bkmk_4_1_Exemptions)|为不需要使用 OTP 进行身份验证的用户规划例外。|  
|[4.2 规划 Windows 7 客户端](#bkmk_4_2_Win7)|计划将 DirectAccess 连接助手（DCA）2.0 部署到 Windows 7 客户端计算机。|  
|[4.3 规划智能卡](#BKMK_smartcard)|规划如何使用智能卡进行额外授权。|  
  
## <a name="bkmk_4_1_Exemptions"></a>4.1 OTP 客户端豁免计划  
启用 OTP 身份验证后，默认情况下，所有用户都需要使用用户名和密码的组合以及 OTP 凭据进行身份验证。 但是，你可以允许选定的用户仅使用用户名和密码进行身份验证，而不使用 OTP。 为此，请创建一个安全组，并添加想要免除 OTP 身份验证的任何用户。  
  
> [!NOTE]  
> 只有单个林中的客户端计算机可能会被免除，这是因为只有一个安全组可以选择进行客户端例外。  
  
## <a name="bkmk_4_2_Win7"></a>4.2 规划 Windows 7 客户端  
默认情况下，Windows 7 客户端计算机不能使用 OTP 进行身份验证。  Windows 7 客户端计算机要求 DCA 2.0 在 Windows Server 2012 远程访问部署中使用 OTP 进行身份验证。 有关 DCA 2.0 的详细信息，请参阅 Microsoft 下载中心上的[DirectAccess 连接助手 2.0](https://go.microsoft.com/fwlink/?LinkId=253699) 。  
  
## <a name="BKMK_smartcard"></a>4.3 规划智能卡  
启用 OTP 身份验证后，可使用启用智能卡的选项进行其他授权。 创建安全组以允许在用户的智能卡不起作用时进行临时访问。  
  
## <a name="BKMK_Links"></a>另请参阅  
  
-   [配置具有 OTP 身份验证的 DirectAccess](https://technet.microsoft.com/windows-server-docs/networking/remote-access/ras/otp/deploy-ra-otp)  
  


