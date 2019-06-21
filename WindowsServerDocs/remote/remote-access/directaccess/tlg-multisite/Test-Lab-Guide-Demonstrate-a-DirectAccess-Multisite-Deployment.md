---
title: 测试实验室指南-演示 DirectAccess 多站点部署
description: 本主题是测试实验室指南的一部分-演示 DirectAccess 多站点部署的 Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3c98106c-67cc-406a-810e-f2e09f7e2c5e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 87c1f629d96d247d273d48066797795b9fe724e6
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281382"
---
# <a name="test-lab-guide-demonstrate-a-directaccess-multisite-deployment"></a>测试实验指南：演示 DirectAccess 多站点部署

>适用于：Windows 服务器 （半年频道），Windows Server 2016

远程访问是使远程用户能够安全地访问内部网络资源使用 DirectAccess 或 RRAS VPN 服务器角色 Windows Server 2016、 Windows Server 2012 R2 和 Windows Server 2012 操作系统中。 本指南包含扩展的分步说明[测试实验室指南：演示混合使用 IPv4 和 IPv6 的 DirectAccess 单个服务器安装](https://go.microsoft.com/fwlink/p/?LinkId=237004)来在多站点方案演示远程访问。  
  
在多站点方案中部署远程访问，可在不同地理位置中配置远程访问服务器。 以前，远程用户需要始终连接到公司网络的特定 DirectAccess 服务器。 使用 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 和 Windows 10 或 Windows 8，您可以在部署中配置每个地理位置的入口点。 每个入口点可以是单个远程访问服务器或远程访问服务器的群集。 远程用户可以选择要连接到任何组织的远程访问入口点。 例如，如果远程用户通常连接到远程访问入口点位于亚洲、 但然后转到欧洲在出差，客户端计算机都将自动连接到最接近的远程访问入口点。  
  
## <a name="about-this-guide"></a>关于本指南  
本指南包含用于配置和演示使用 9 个服务器和三个客户端计算机的远程访问的说明。 已完成远程访问多站点测试实验室模拟 intranet、 Internet 和家庭网络，并演示了在不同的 Internet 连接方案中的远程访问功能。  
  
> [!IMPORTANT]  
> 此实验室是一个使用最小量计算机的概述证明。 本指南中详细介绍的配置仅适用于测试实验室目的，不可用于生产环境。  
  


