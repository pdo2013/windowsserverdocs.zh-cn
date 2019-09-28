---
title: 步骤4通过 OTP 验证 DirectAccess
description: 本主题是指南使用 Windows Server 2016 中的 "使用 OTP 身份验证部署远程访问" 指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed49a0a3-1c45-42e5-8f13-cad20c1c1d68
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 83ea3c4e4feefacde3e1ed7be6b605d8c0e644a3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366971"
---
# <a name="step-4-verify-directaccess-with-otp"></a>步骤4通过 OTP 验证 DirectAccess

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题介绍如何使用 OTP 部署来验证是否已正确配置 DirectAccess。
  
### <a name="to-verify-otp-health-on-the-remote-access-server"></a>验证远程访问服务器上的 OTP 运行状况

1. 在远程访问服务器上，打开**远程访问管理**控制台。  

2. 在 "**远程访问服务器**" 下，单击已配置为支持 OTP 的远程访问服务器。  

3. 单击 "**操作状态**"。  

4. 验证 "OTP" 的状态是否显示绿色图标并且是否正常工作。  
  
    > [!NOTE]  
    > 运行状况状态更新间隔将是注册表项 HKLM\SYSTEM\CCS\Services\Ramgmtsvc\parameters\HealthRefreshTimeout 中的值的总和，以及远程访问中设置的**服务器活动的时间间隔。** configuration.  
  
### <a name="to-verify-access-to-internal-resources-using-otp-authentication"></a>使用 OTP 身份验证验证对内部资源的访问  
  
1.  将 DirectAccess 客户端计算机连接到公司网络，然后从命令提示符运行**gpupdate/force**以获取组策略。  
  
2.  断开客户端计算机与企业网络的连接，连接到外部网络并尝试访问内部资源。 您不应该有权访问内部资源。  
  
3.  对于软件令牌，请使用供应商说明访问 OTP 客户端令牌，并记下当前令牌代码。 使用硬件令牌时，请按照供应商说明进行身份验证。  
  
4.  单击通知区域中的“网络连接” 图标以访问 DA 媒体管理器。  
  
5.  单击 " **DirectAccess 连接**"，然后单击 "**继续**"。  
  
6.  输入前面记下的令牌代码，然后单击 **"确定"** 。 等待身份验证完成。 此时将**连接**DirectAccess 工作区连接状态。  
  
7.  尝试访问内部资源。 你应能够访问所有公司资源。  
  


