---
title: 使用 Windows PowerShell 配置非域成员客户端计算机
description: 本主题是 BranchCache 部署指南为 Windows Server 2016 中，该示例演示了如何部署 BranchCache 在分布式和托管缓存模式下以优化分支机构中的 WAN 带宽使用情况的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 1b511e1a-686d-441f-a1c7-d4d029e1a061
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9abb77e573d7b3f144ab831c655c81370a4a6af1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839948"
---
# <a name="use-windows-powershell-to-configure-non-domain-member-client-computers"></a>使用 Windows PowerShell 配置非域成员客户端计算机

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以使用此过程来手动配置 BranchCache 客户端计算机用于分布式的缓存模式或托管缓存模式。  
  
> [!NOTE]  
> 如果已配置 BranchCache 客户端计算机使用组策略，组策略设置将覆盖客户端计算机将策略应用到的任何手动配置。  
  
中的成员身份**管理员**，或等效身份是执行此过程所需的最低。  
  
### <a name="to-enable-branchcache-distributed-or-hosted-cache-mode"></a>若要启用 BranchCache 分布式或托管缓存模式  
  
1.  在你想要配置 BranchCache 客户端计算机，以管理员身份运行 Windows PowerShell，然后执行下列任一。  
  
    -   若要配置为 BranchCache 分布式的缓存模式客户端计算机，键入以下命令，然后按 ENTER。  
  
        `Enable-BCDistributed`  
  
    -   若要配置为 BranchCache 托管缓存模式客户端计算机，键入以下命令，然后按 ENTER。  
  
        `Enable-BCHostedClient`  
  
        > [!TIP]  
        > 如果你想要指定可用的托管的缓存服务器，使用`-ServerNames`参数使用逗号分隔的托管的缓存服务器作为参数值的列表。 例如，如果您有两个名为 HCS1 和 HCS2 的托管的缓存服务器，则可配置为托管的缓存模式客户端计算机使用以下命令。  
        >   
        > `Enable-BCHostedClient -ServerNames HCS1,HCS2`  
  


