---
title: 安装分支缓存功能
description: 本主题介绍 Windows Server 2016，其中演示如何在分布式托管的缓存模式优化分支机构中 WAN 带宽使用量部署分支缓存分支缓存部署指南中
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 4f31dc61-2dbe-4c7e-b3f9-85ae49a45049
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a69848536b56521da9b5ef07689aba7f8690e888
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="install-the-branchcache-feature"></a>安装分支缓存功能

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用此过程安装分支缓存的功能，并运行 Windows Server 的计算机上启动分支缓存服务&reg;2016，Windows Server 2012 R2 或 Windows Server 2012。  
  
在会员**管理员**或等效的最低要求执行此过程。  
  
执行此过程之前，建议您安装和配置你基于位的应用程序或 Web 服务器。  
  
> [!NOTE]  
> 若要执行此过程，方法是使用 Windows PowerShell，Windows PowerShell 管理员身份运行，在 Windows PowerShell 提示符下，键入以下命令，然后按 ENTER。  
>   
> `Install-WindowsFeature BranchCache`  
>   
> `Restart-Computer`  
  
### <a name="to-install-and-enable-the-branchcache-feature"></a>安装并启用分支缓存功能  
  
1.  在服务器管理器中，单击**管理**，然后单击**添加角色和功能**。 添加角色和功能向导将打开。 单击**下一步**。  
  
2.  在**选择安装类型**，确保**角色基于或功能的安装**选中，则，然后单击**下一步**。  
  
3.  在**选择目标服务器**，确保正确的服务器选择，然后单击**下一步**。  
  
4.  在**选择服务器角色**，单击**下一步**。  
  
5.  在**选择功能**，单击**分支缓存**，然后单击**下一步**。  
  
6.  在**确认安装选择**，单击**安装**。 在**安装进度**，则分支缓存功能继续安装。 安装完成后，单击**关闭**。  
  
安装分支缓存功能后，分支缓存服务-也称为 PeerDistSvc-已启用，并开始键入为自动。  
  


