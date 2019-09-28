---
title: 步骤2配置 RADIUS 服务器
description: 本主题是指南使用 Windows Server 2016 中的 "使用 OTP 身份验证部署远程访问" 指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0326818f-9144-496c-b946-f82be4eefbd3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 00ea76d6995f875e509a3bc9ef0bab3d2689c52b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367018"
---
# <a name="step-2-configure-the-radius-server"></a>步骤2配置 RADIUS 服务器

>适用于：Windows Server（半年频道）、Windows Server 2016

在配置远程访问服务器以支持支持 OTP 的 DirectAccess 之前，请配置 RADIUS 服务器。  
  
|任务|描述|  
|----|--------|  
|[2.1。配置 RADIUS 软件分发令牌 @ no__t-0|在 RADIUS 服务器上配置软件分发令牌。|  
|[2.2。配置 RADIUS 安全信息 @ no__t-0|在 RADIUS 服务器上配置要使用的端口和共享机密。|  
|[2.3 添加用于 OTP 探测的用户帐户](#BKMK_Probe)|在 RADIUS 服务器上，为 OTP 探测创建新的用户帐户。|  
|[2.4 与 Active Directory 同步](#BKMK_Active)|在 RADIUS 服务器上，创建与 Active Directory 帐户同步的用户帐户。|  
|[2.5 配置 RADIUS 身份验证代理](#BKMK_AuthAgent)|将远程访问服务器配置为 RADIUS 身份验证代理。|  
  
## <a name="BKMK_1.1"></a>2.1 配置 RADIUS 软件分发令牌  
必须使用所需的许可证和软件和/或硬件分发令牌配置 RADIUS 服务器，DirectAccess 才能使用 OTP。 此过程特定于每个 RADIUS 供应商实现。  
  
## <a name="BKMK_1.2"></a>2.2 配置 RADIUS 安全信息  
RADIUS 服务器出于通信目的使用 UDP 端口，每个 RADIUS 供应商都有其自己的默认 UDP 端口用于传入和传出通信。 要使 RADIUS 服务器与远程访问服务器一起工作，请确保环境中的所有防火墙都已配置为允许 DirectAccess 与 OTP 服务器之间的 UDP 流量根据需要通过所需端口进行通信。  
  
RADIUS 服务器使用共享机密进行身份验证。 使用共享机密的强密码配置 RADIUS 服务器，并注意在配置 DirectAccess 服务器的客户端计算机配置以便与具有 OTP 的 DirectAccess 一起使用时，将使用此服务器。  
  
## <a name="BKMK_Probe"></a>2.3 添加用于 OTP 探测的用户帐户  
在 RADIUS 服务器上，创建名为**DAProbeUser**的新用户帐户，并为其指定密码**DAProbePass**。  
  
## <a name="BKMK_Active"></a>2.4 与 Active Directory 同步  
RADIUS 服务器必须具有与 Active Directory 中将使用 DirectAccess 和 OTP 的用户相对应的用户帐户。  
  
#### <a name="to-synchronize-the-radius-and-active-directory-users"></a>同步 RADIUS 和 Active Directory 用户  
  
1.  记录包含 OTP 用户的所有 DirectAccess 的 Active Directory 中的用户信息。  
  
2.  使用供应商特定的过程在所记录的 RADIUS 服务器中创建相同的用户**域 \ 用户名**帐户。  
  
## <a name="BKMK_AuthAgent"></a>2.5 配置 RADIUS 身份验证代理  
远程访问服务器必须配置为具有 OTP 实现的 DirectAccess 的 RADIUS 身份验证代理。 遵循 RADIUS 供应商说明，将远程访问服务器配置为 RADIUS 身份验证代理。  
  


