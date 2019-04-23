---
title: 安装 BranchCache 功能
description: 本主题是 BranchCache 部署指南为 Windows Server 2016 中，该示例演示了如何部署 BranchCache 在分布式和托管缓存模式下以优化分支机构中的 WAN 带宽使用情况的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 4f31dc61-2dbe-4c7e-b3f9-85ae49a45049
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8b4aecd9e9355a6c2d5ac485ac77c76428fe295f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872188"
---
# <a name="install-the-branchcache-feature"></a>安装 BranchCache 功能

>适用于：Windows 服务器 （半年频道），Windows Server 2016

此过程可用于安装 BranchCache 功能并运行 Windows Server 的计算机上启动 BranchCache 服务&reg;2016 中，Windows Server 2012 R2 或 Windows Server 2012。  
  
中的成员身份**管理员**或等效身份是执行此过程所需的最低。  
  
在执行此过程之前，建议您安装和配置基于 BITS 的应用程序或 Web 服务器。  
  
> [!NOTE]  
> 若要使用 Windows PowerShell，以管理员身份运行 Windows PowerShell 执行此过程在 Windows PowerShell 提示符下键入以下命令，然后按 ENTER。  
>   
> `Install-WindowsFeature BranchCache`  
>   
> `Restart-Computer`  
  
### <a name="to-install-and-enable-the-branchcache-feature"></a>若要安装并启用 BranchCache 功能  
  
1.  在“服务器管理器”中，单击“管理”，然后单击“添加角色和功能”。 添加角色和功能向导将打开。 单击“下一步” 。  
  
2.  在中**选择安装类型**，确保**基于角色或基于功能的安装**已选择，然后单击**下一步**。  
  
3.  在中**选择目标服务器**，确保正确的服务器选择，然后单击**下一步**。  
  
4.  在“选择服务器角色” 中，单击“下一步” 。  
  
5.  在中**选择的功能**，单击**BranchCache**，然后单击**下一步**。  
  
6.  在 **“确认安装选择”** 中，单击 **“安装”**。 在中**安装进度**，BranchCache 功能安装过程继续进行。 安装完成后，单击**关闭**。  
  
安装 BranchCache 功能后，启用 BranchCache 服务-也称为 PeerDistSvc-，并为自动启动类型。  
  


