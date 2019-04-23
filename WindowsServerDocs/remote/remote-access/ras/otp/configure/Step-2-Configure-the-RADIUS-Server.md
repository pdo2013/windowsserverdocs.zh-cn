---
title: 步骤 2 配置 RADIUS 服务器
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
ms.assetid: 0326818f-9144-496c-b946-f82be4eefbd3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 219c333745d28bdedb9027c1dd46148ac80998f6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840868"
---
# <a name="step-2-configure-the-radius-server"></a>步骤 2 配置 RADIUS 服务器

>适用于：Windows 服务器 （半年频道），Windows Server 2016

配置远程访问服务器以支持之前与 OTP 的 DirectAccess 支持，则配置 RADIUS 服务器。  
  
|任务|描述|  
|----|--------|  
|[2.1.配置 RADIUS 软件分发令牌](#BKMK_1.1)|RADIUS 服务器上配置分发的软件令牌。|  
|[2.2.配置 RADIUS 安全信息](#BKMK_1.2)|RADIUS 服务器上配置的端口和要使用的共享的机密。|  
|[2.3 添加探测 OTP 的用户帐户](#BKMK_Probe)|在 RADIUS 服务器上创建新的用户帐户用于 OTP 探测。|  
|[2.4 与 Active Directory 同步](#BKMK_Active)|在 RADIUS 服务器上创建与 Active Directory 帐户同步的用户帐户。|  
|[2.5 配置 RADIUS 身份验证代理](#BKMK_AuthAgent)|将远程访问服务器配置为 RADIUS 身份验证代理。|  
  
## <a name="BKMK_1.1"></a>2.1 配置 RADIUS 软件分发令牌  
必须使用必要的许可证和软件和/或硬件分发令牌以供使用 OTP 的 DirectAccess 配置 RADIUS 服务器。 此过程将特定于每个 RADIUS 供应商实现。  
  
## <a name="BKMK_1.2"></a>2.2 配置 RADIUS 安全信息  
RADIUS 服务器使用 UDP 端口进行通信目的和每个 RADIUS 供应商都有其自己是默认 UDP 端口进行传入和传出通信。 为 RADIUS 服务器使用的远程访问服务器，请确保在环境中的所有防火墙都配置为根据需要允许通过所需的端口在 DirectAccess 和 OTP 服务器之间的 UDP 流量。  
  
RADIUS 服务器进行身份验证，使用共享的机密。 配置 RADIUS 服务器的共享机密的强密码并请注意，此文件用于使用带有 OTP 的 DirectAccess 配置为使用 DirectAccess 服务器的客户端计算机配置时。  
  
## <a name="BKMK_Probe"></a>2.3 添加探测 OTP 的用户帐户  
在 RADIUS 服务器上创建名为的新用户帐户**DAProbeUser**并为其提供密码**DAProbePass**。  
  
## <a name="BKMK_Active"></a>2.4 与 Active Directory 同步  
RADIUS 服务器必须具有对应于要使用 DirectAccess OTP 的 Active Directory 中用户的用户帐户。  
  
#### <a name="to-synchronize-the-radius-and-active-directory-users"></a>若要将 RADIUS 和 Active Directory 用户同步  
  
1.  记录的用户信息从 Active Directory 为所有 DirectAccess OTP 的用户使用。  
  
2.  使用供应商特定过程来创建相同的用户**域 \ 用户名**RADIUS 服务器记录的帐户。  
  
## <a name="BKMK_AuthAgent"></a>2.5 配置 RADIUS 身份验证代理  
远程访问服务器必须配置为使用 OTP 实现 DirectAccess 在 RADIUS 身份验证代理。 按照 RADIUS 供应商说明进行操作，将远程访问服务器配置为 RADIUS 身份验证代理。  
  


