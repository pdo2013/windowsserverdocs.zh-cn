---
title: OTP 的远程访问服务器上的第 4 步计划
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
ms.assetid: 4b97b2fd-767a-45c1-a64e-5b3edd0c8a47
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 470eebc82177a21985afb8d0bf143427a33d65fb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825438"
---
# <a name="step-4-plan-for-otp-on-the-remote-access-server"></a>OTP 的远程访问服务器上的第 4 步计划

>适用于：Windows 服务器 （半年频道），Windows Server 2016

在计划的一次性密码 (OTP) RADIUS 服务器和证书设置之后, 在规划远程访问 OTP 部署中的最后一步是规划远程访问服务器上的客户端 OTP 设置。  
  
|任务|描述|  
|----|--------|  
|[4.1 规划客户端 OTP 免除](#bkmk_4_1_Exemptions)|规划你不需要使用 OTP 身份验证的用户的例外。|  
|[4.2 规划 Windows 7 客户端](#bkmk_4_2_Win7)|计划部署到 Windows 7 客户端计算机的 DirectAccess 连接助手 (DCA) 2.0。|  
|[4.3 规划智能卡](#BKMK_smartcard)|规划将智能卡进行额外授权。|  
  
## <a name="bkmk_4_1_Exemptions"></a>4.1 规划客户端 OTP 免除  
启用 OTP 身份验证后，默认情况下所有要求用户进行身份验证的用户名和密码，输入 OTP 凭据结合使用。 但是，可以允许选定的用户使用的用户名和密码，而无需 OTP 进行身份验证。 若要执行此操作，创建一个安全组，并将所需的任何用户添加到免除 OTP 身份验证。  
  
> [!NOTE]  
> 只能从单个林的客户端计算机可能会由于这一事实免除该只有一个安全组可用于客户端例外。  
  
## <a name="bkmk_4_2_Win7"></a>4.2 规划 Windows 7 客户端  
默认情况下，Windows 7 客户端计算机不能使用 OTP 身份验证。  Windows 7 客户端计算机需要 DCA 2.0 以 Windows Server 2012 远程访问部署中使用 OTP 进行身份验证。 有关 DCA 2.0 的详细信息，请参阅[DirectAccess Connectivity Assistant 2.0](https://go.microsoft.com/fwlink/?LinkId=253699)从 Microsoft 下载中心获得。  
  
## <a name="BKMK_smartcard"></a>4.3 规划智能卡  
启用 OTP 身份验证后，若要启用智能卡用于其他授权选项才可用。 创建安全组允许的临时访问权限的用户的智能卡不起作用。  
  
## <a name="BKMK_Links"></a>另请参阅  
  
-   [带有 OTP 身份验证配置 DirectAccess](https://technet.microsoft.com/windows-server-docs/networking/remote-access/ras/otp/deploy-ra-otp)  
  


