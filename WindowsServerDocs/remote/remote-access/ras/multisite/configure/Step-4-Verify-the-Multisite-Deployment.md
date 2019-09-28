---
title: 步骤4验证多站点部署
description: 本主题是在 Windows Server 2016 中的多站点部署中部署多台远程访问服务器指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 345b676a-a397-4d51-9973-8b25bc05fa55
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3574ef57d18e23668f08dee8b768f0114790f0b8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367126"
---
# <a name="step-4-verify-the-multisite-deployment"></a>步骤4验证多站点部署

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题介绍如何验证是否已正确配置远程访问多站点部署。  
  
### <a name="to-verify-access-to-internal-resources-through-the-multisite-deployment"></a>通过多站点部署验证对内部资源的访问  
  
1.  将 DirectAccess 客户端计算机连接到公司网络并获取组策略。  
  
2.  将客户端计算机连接到外部网络并尝试访问内部资源。  
  
    你应能够访问所有公司资源。  
  
3.  在多站点部署中通过每个服务器来测试连接，方法是关闭外部网络，或从外部网络断开连接，而不是所有远程访问服务器。 在客户端计算机上，尝试访问公司资源。 对其他多站点服务器重复此测试。 客户端计算机连接到新的入口点可能需要长达10分钟的时间。 这是因为在将入口点视为不可访问状态之后，探测会关闭10分钟，以便优化带宽和电池寿命。 另外，还可以通过在执行**daprop**时显示的组合框中选择所需的入口点，手动在各个入口点之间切换。  
  
    应该能够通过每个多站点服务器访问所有公司资源。  
  
4.  将 Windows 7 @ no__t-0 客户端计算机连接到公司网络并获取组策略。  
  
5.  将 Windows 7 客户端计算机连接到外部网络并尝试访问内部资源。  
  
    你应能够访问所有公司资源。  
  
6.  通过多站点部署中的每个服务器测试 Windows 7 客户端的连接，方法是访问 "Active Directory 用户和计算机" 控制台，并将客户端计算机移动到对应于每个服务器的安全组。 在整个域中复制更改后，在连接到公司网络时重新启动客户端计算机，以获取新的组策略。 尝试访问公司资源。 对其他多站点服务器重复此测试。  
  
    应该能够通过每个多站点服务器访问所有公司资源。  
  
    在生产环境中，此方法可能不可行，因为在整个域中复制更改所需的时间量。 可能需要强制进行复制。 还可以通过多个不同的 Windows 7 客户端计算机执行测试，这些计算机已是多站点部署中的不同 Windows 7 安全组的成员。  
  


