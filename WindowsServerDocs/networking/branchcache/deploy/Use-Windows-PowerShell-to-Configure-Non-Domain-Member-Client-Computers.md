---
title: 使用 Windows PowerShell 配置非域成员客户端计算机
description: 本主题是适用于 Windows Server 2016 的 BranchCache 部署指南的一部分，它演示了如何在分布式和托管缓存模式下部署 BranchCache，以优化分支机构中的 WAN 带宽使用情况
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 1b511e1a-686d-441f-a1c7-d4d029e1a061
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9743d93fe7bc21a971ff886a7e255eed3b775c97
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406425"
---
# <a name="use-windows-powershell-to-configure-non-domain-member-client-computers"></a>使用 Windows PowerShell 配置非域成员客户端计算机

>适用于：Windows Server（半年频道）、Windows Server 2016

你可以使用此过程为分布式缓存模式或托管缓存模式手动配置 BranchCache 客户端计算机。  
  
> [!NOTE]  
> 如果已使用组策略配置 BranchCache 客户端计算机，则组策略设置将覆盖应用策略的客户端计算机的任何手动配置。  
  
**Administrators**中的成员身份或同等身份是执行此过程所需的最低要求。  
  
### <a name="to-enable-branchcache-distributed-or-hosted-cache-mode"></a>启用 BranchCache 分布式或托管缓存模式  
  
1.  在要配置的 BranchCache 客户端计算机上，以管理员身份运行 Windows PowerShell，然后执行以下操作之一。  
  
    -   若要为 BranchCache 分布式缓存模式配置客户端计算机，请键入以下命令，然后按 ENTER。  
  
        `Enable-BCDistributed`  
  
    -   若要为 BranchCache 托管缓存模式配置客户端计算机，请键入以下命令，然后按 ENTER。  
  
        `Enable-BCHostedClient`  
  
        > [!TIP]  
        > 如果要指定可用的托管缓存服务器，请将 `-ServerNames` 参数与托管缓存服务器的逗号分隔列表作为参数值一起使用。 例如，如果有两个名为 HCS1 和 HCS2 的托管缓存服务器，请使用以下命令为托管缓存模式配置客户端计算机。  
        >   
        > `Enable-BCHostedClient -ServerNames HCS1,HCS2`  
  


