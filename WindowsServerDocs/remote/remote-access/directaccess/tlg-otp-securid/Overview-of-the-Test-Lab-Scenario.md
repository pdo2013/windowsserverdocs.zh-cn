---
title: 测试实验室方案概述
description: 本主题是测试实验室指南的一部分-演示带有 OTP 身份验证的 DirectAccess 和用于 Windows Server 2016 的 RSA SecurID
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ce584811-b209-48fe-ab2b-4c399bd0bd79
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ba3981c79d1441797f21c8ed8051ace6ac490cab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404733"
---
# <a name="overview-of-the-test-lab-scenario"></a>测试实验室方案概述

>适用于：Windows Server（半年频道）、Windows Server 2016

远程访问是 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 操作系统中的一种服务器角色，使远程用户能够使用 DirectAccess 或虚拟专用网络（Vpn）安全访问内部网络资源，路由和远程访问服务（RRAS）。 本指南包含扩展 @no__t 实验室指南的分步说明：使用混合的 IPv4 和 IPv6 @ no__t 演示 DirectAccess 单服务器安装程序，以演示远程访问一次性密码（OTP）配置。  
  
> [!WARNING]  
> 本测试实验室指南的设计包括基础结构服务器，如运行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 的域控制器和证书颁发机构（CA）。 本指南中未介绍如何使用此测试实验室指南来配置运行其他操作系统的基础结构服务器，以及配置其他操作系统的说明。  
  
## <a name="about-this-guide"></a>关于本指南  
Windows server 2016 中的远程访问，Windows Server 2012 R2 和 Windows Server 2012 添加了对使用 OTP 的客户端身份验证的支持。 在此测试实验室中，仅 RSA SecurID 用于演示使用远程访问的 OTP 功能。 还支持其他基于 RADIUS 的 OTP 解决方案，但超出了本测试实验室的范围。 本指南包含使用六个服务器和两个客户端计算机配置和演示远程访问的指导。 使用 OTP 测试实验室完成的远程访问模拟 intranet、Internet 和家庭网络，并演示了不同 Internet 连接方案中的远程访问功能。  
  
> [!IMPORTANT]  
> 此实验室是一个使用最小量计算机的概述证明。 本指南中详细介绍的配置仅适用于测试实验室目的，不可用于生产环境。  
  


