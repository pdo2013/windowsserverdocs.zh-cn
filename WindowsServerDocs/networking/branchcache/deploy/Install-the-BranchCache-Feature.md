---
title: 安装 BranchCache 功能
description: 本主题是适用于 Windows Server 2016 的 BranchCache 部署指南的一部分，它演示了如何在分布式和托管缓存模式下部署 BranchCache，以优化分支机构中的 WAN 带宽使用情况
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 4f31dc61-2dbe-4c7e-b3f9-85ae49a45049
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5ee438ef57d3355cf19713d8574591aeea6ae06f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406432"
---
# <a name="install-the-branchcache-feature"></a>安装 BranchCache 功能

>适用于：Windows Server（半年频道）、Windows Server 2016

你可以使用此过程在运行 Windows Server @ no__t-0 2016、Windows Server 2012 R2 或 Windows Server 2012 的计算机上安装 BranchCache 功能并启动 BranchCache 服务。  
  
**Administrators**中的成员身份或同等身份是执行此过程所需的最低要求。  
  
在执行此过程之前，建议安装并配置基于 BITS 的应用程序或 Web 服务器。  
  
> [!NOTE]  
> 若要使用 Windows PowerShell 执行此过程，请以管理员身份运行 Windows PowerShell，在 Windows PowerShell 提示符下键入以下命令，然后按 ENTER。  
>   
> `Install-WindowsFeature BranchCache`  
>   
> `Restart-Computer`  
  
### <a name="to-install-and-enable-the-branchcache-feature"></a>安装和启用 BranchCache 功能  
  
1.  在“服务器管理器”中，单击“管理”，然后单击“添加角色和功能”。 "添加角色和功能向导" 将打开。 单击“下一步”。  
  
2.  在 "**选择安装类型**" 中，确保选择了 "**基于角色或基于功能的安装**"，然后单击 "**下一步**"。  
  
3.  在 "**选择目标服务器**" 中，确保选择了正确的服务器，然后单击 "**下一步**"。  
  
4.  在“选择服务器角色”中，单击“下一步”。  
  
5.  在 "**选择功能**" 中，单击 " **BranchCache**"，然后单击 "**下一步**"。  
  
6.  在 **“确认安装选择”** 中，单击 **“安装”** 。 在**安装进度**下，BranchCache 功能安装将继续进行。 安装完成后，单击 "**关闭**"。  
  
安装 BranchCache 功能后，BranchCache 服务（也称为 PeerDistSvc）已启用，并且启动类型为 "自动"。  
  


