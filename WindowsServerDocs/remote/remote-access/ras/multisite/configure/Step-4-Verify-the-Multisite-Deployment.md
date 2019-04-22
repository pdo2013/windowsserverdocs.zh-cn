---
title: 步骤 4 验证多站点部署
description: 本主题是指南的一部分部署多台远程访问服务器在 Windows Server 2016 中的多站点部署中。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 345b676a-a397-4d51-9973-8b25bc05fa55
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 17b944bd0e2c13f9a3d324eeda09c67b110ce49d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821848"
---
# <a name="step-4-verify-the-multisite-deployment"></a>步骤 4 验证多站点部署

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题介绍如何验证正确配置了远程访问多站点部署。  
  
### <a name="to-verify-access-to-internal-resources-through-the-multisite-deployment"></a>若要验证可以访问内部资源通过多站点部署  
  
1.  将 DirectAccess 客户端计算机连接到公司网络并获取组策略。  
  
2.  将客户端计算机连接到外部网络并尝试访问内部资源。  
  
    你应能够访问所有公司资源。  
  
3.  通过关闭，或从外部网络，一个远程访问服务器断开连接在多站点部署中测试通过的每个服务器的连接。 在客户端计算机上，尝试访问公司资源。 重复该测试不同的多站点服务器上。 可能需要 10 分钟的客户端计算机连接到新的入口点。 这是因为探测处于关闭状态的入口点为 10 分钟后将其视为无法访问，以优化带宽和电池寿命。 或者，您可以通过从执行时，显示组合框中选择所需的入口点进行手动配置各个入口点之间切换**daprop.exe**。  
  
    您应能够通过每个多站点服务器中访问所有公司资源。  
  
4.  连接 Windows 7&reg;客户端计算机添加到公司网络并获取组策略。  
  
5.  Windows 7 客户端计算机连接到外部网络并尝试访问内部资源。  
  
    你应能够访问所有公司资源。  
  
6.  访问 Active Directory 用户和计算机控制台中，并将客户端计算机移动到对应于每个服务器的安全组来测试 Windows 7 客户端可以通过在多站点部署中每个服务器连接。 所做的更改已复制到整个域后，重新启动客户端计算机连接到公司网络，以便获取新的组策略时。 尝试访问公司资源。 重复该测试不同的多站点服务器上。  
  
    您应能够通过每个多站点服务器中访问所有公司资源。  
  
    此方法可能可行，整个域中复制更改所需的时间，因为在生产环境中。 您可能想要强制复制，在可能的情况。 也可以从多个不同 Windows 7 客户端计算机已在多站点部署中的不同 Windows 7 安全组的成员的测试。  
  


