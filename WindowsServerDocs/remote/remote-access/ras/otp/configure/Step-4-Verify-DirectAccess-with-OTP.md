---
title: 步骤 4 验证使用 OTP 的 DirectAccess
description: 本主题是指南部署带有 OTP 身份验证，Windows Server 2016 中的远程访问的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed49a0a3-1c45-42e5-8f13-cad20c1c1d68
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1ce9fe1327cfad6409d66981e6baadc133fb92a6
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282411"
---
# <a name="step-4-verify-directaccess-with-otp"></a>步骤 4 验证使用 OTP 的 DirectAccess

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题介绍如何验证已正确配置在 DirectAccess OTP 部署。
  
### <a name="to-verify-otp-health-on-the-remote-access-server"></a>若要验证远程访问服务器上的 OTP 运行状况

1. 打开远程访问服务器上**远程访问管理**控制台。  

2. 下**远程访问服务器**单击已配置为支持 OTP 的远程访问服务器。  

3. 单击**操作状态**。  

4. 验证的 OTP 状态显示绿色图标和工作。  
  
    > [!NOTE]  
    > 运行状况状态更新间隔将从注册表项 HKLM\SYSTEM\CCS\Services\Ramgmtsvc\parameters\HealthRefreshTimeout 值之和的最大值和**服务器的活动的时间间隔**中设置远程访问配置。  
  
### <a name="to-verify-access-to-internal-resources-using-otp-authentication"></a>若要验证使用 OTP 身份验证的内部资源的访问权限  
  
1.  DirectAccess 客户端计算机连接到公司网络并运行**gpupdate /force**从命令提示符下，若要获取组策略。  
  
2.  断开客户端计算机与公司网络的连接，连接到外部网络，并尝试访问内部资源。 不应具有访问内部资源。  
  
3.  对于的软件令牌，访问 OTP 客户端令牌使用供应商的说明，并记下当前令牌的代码。 使用硬件令牌时，请按照供应商说明进行身份验证。  
  
4.  单击通知区域中的“网络连接”  图标以访问 DA 媒体管理器。  
  
5.  单击**DirectAccess 连接**，然后单击**继续**。  
  
6.  输入前面，记下的令牌代码，然后单击**确定**。 等待要完成身份验证。 DirectAccess 工作区连接状态现在将**已连接**。  
  
7.  尝试访问内部资源。 你应能够访问所有公司资源。  
  


