---
title: 安装新的文件服务器作为内容服务器
description: 本主题是适用于 Windows Server 2016 的 BranchCache 部署指南的一部分，它演示了如何在分布式和托管缓存模式下部署 BranchCache，以优化分支机构中的 WAN 带宽使用情况
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 1f49fc3c-28a6-4d3d-b787-1be9e61e792f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 489006c50ccbfa1f452d56b1a18217692d45cb1f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406453"
---
# <a name="install-a-new-file-server-as-a-content-server"></a>安装新的文件服务器作为内容服务器

>适用于：Windows Server（半年频道）、Windows Server 2016

在运行 Windows Server 2016 的计算机上，您可以使用此过程安装 "文件服务" 服务器角色和 "**网络文件 BranchCache** " 角色服务。  
  
**Administrators**中的成员身份或同等身份是执行此过程所需的最低要求。  
  
> [!NOTE]  
> 若要使用 Windows PowerShell 执行此过程，请以管理员身份运行 Windows PowerShell，在 Windows PowerShell 提示符下键入以下命令，然后按 ENTER。  
>   
> `Install-WindowsFeature FS-BranchCache -IncludeManagementTools`  
>   
> `Restart-Computer`  
>   
> 若要安装重复数据删除角色服务，请键入以下命令，然后按 ENTER。  
>   
> `Install-WindowsFeature FS-Data-Deduplication -IncludeManagementTools`  
  
### <a name="to-install-file-services-and-the-branchcache-for-network-files-role-service"></a>安装文件服务和网络文件的 BranchCache 角色服务  
  
1.  在“服务器管理器”中，单击“管理”，然后单击“添加角色和功能”。 将打开“添加角色和功能向导”。 在“开始之前”中，单击“下一步”。  
  
2.  在 "**选择安装类型**" 中，确保选择了 "**基于角色或基于功能的安装**"，然后单击 "**下一步**"。  
  
3.  在 "**选择目标服务器**" 中，确保选择了正确的服务器，然后单击 "**下一步**"。  
  
4.  在 "**选择服务器角色**" 的 "**角色**" 中，请注意，已安装**文件和存储服务**角色;单击角色名称左侧的箭头以展开 "角色服务" 选择，然后单击 "**文件和 ISCSI 服务**" 左侧的箭头。  
  
5.  选中 "**文件服务器**" 和 "**网络文件 BranchCache**" 对应的复选框。  
  
    > [!TIP]  
    > 建议还选中 "**重复数据删除**" 复选框。
  
    单击“下一步”。  
  
6.  在 "**选择功能**" 中，单击 "**下一步**"。  
  
7.  在 "**确认安装选择**" 中，检查你的选择，然后单击 "**安装**"。 安装过程中会显示 "**安装进度**" 窗格。 安装完成后，单击 "**关闭**"。
