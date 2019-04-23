---
title: 测试实验室指南-演示带有 OTP 身份验证和 RSA SecurID 的 DirectAccess
description: 本主题是一部分的测试实验室指南-使用 OTP 身份验证和 Windows Server 2016 的 RSA SecurID 演示 DirectAccess
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 10c7a49c-5671-4bec-b562-13fdd67f4629
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a6e4b03ceb331899ac622bffe44061a7d92f5a09
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866228"
---
# <a name="test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid"></a>测试实验指南：演示带有 OTP 身份验证和 RSA SecurID 的 DirectAccess

>适用于：Windows 服务器 （半年频道），Windows Server 2016

远程访问是 Windows Server 2016、 Windows Server 2012 R2 和 Windows Server 2012 操作系统，使远程用户能够安全地访问内部网络资源与路由使用 DirectAccess 或虚拟专用网络 (Vpn) 中的服务器角色和远程访问服务 (RRAS)。 本指南包含扩展的分步说明[测试实验室指南：演示混合使用 IPv4 和 IPv6 的 DirectAccess 单个服务器安装](https://go.microsoft.com/fwlink/p/?LinkId=237004)演示远程访问的一次性密码 (OTP) 配置。  
  
> [!WARNING]  
> 本测试实验室指南的设计包括基础结构服务器，如域控制器和运行的 Windows Server 2012 R2 或 Windows Server 2012 的证书颁发机构 (CA)。 使用本测试实验室指南来配置正在运行其他操作系统的基础结构服务器尚未经过测试，并配置其他操作系统的说明不属于本指南中。  
  
## <a name="about-this-guide"></a>关于本指南  
Windows Server 2016、 Windows Server 2012 R2 和 Windows Server 2012 中的远程访问添加了对客户端使用 OTP 进行身份验证支持。 出于本测试实验室中的 RSA SecurID 仅用于与远程访问演示 OTP 功能。 基于其他 RADIUS 的 OTP 解决方案，支持，但此测试实验室的作用域外。 本指南包含使用六个服务器和两个客户端计算机配置和演示远程访问的指导。 使用 OTP 测试实验室已完成的远程访问模拟 intranet、 Internet 和家庭网络，并演示了在不同的 Internet 连接方案中的远程访问功能。  
  
> [!IMPORTANT]  
> 此实验室是一个使用最小量计算机的概述证明。 本指南中详细介绍的配置仅适用于测试实验室目的，不可用于生产环境。  
  


