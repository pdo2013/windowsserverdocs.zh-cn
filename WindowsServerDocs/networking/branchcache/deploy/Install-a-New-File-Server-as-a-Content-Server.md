---
title: 将新文件服务器，在安装为内容服务器
description: 本主题介绍 Windows Server 2016，其中演示如何在分布式托管的缓存模式优化分支机构中 WAN 带宽使用量部署分支缓存分支缓存部署指南中
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 1f49fc3c-28a6-4d3d-b787-1be9e61e792f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 53937cfc139efc6df5facfa872e63609229a548c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="install-a-new-file-server-as-a-content-server"></a>将新文件服务器，在安装为内容服务器

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用此过程安装文件服务 server 角色和**网络文件的分支缓存**角色运行 Windows Server 2016 的计算机上的服务。  
  
在会员**管理员**，或等效的最低要求执行此过程。  
  
> [!NOTE]  
> 若要执行此过程，方法是使用 Windows PowerShell，Windows PowerShell 管理员身份运行，在 Windows PowerShell 提示符下，键入以下命令，然后按 ENTER。  
>   
> `Install-WindowsFeature FS-BranchCache -IncludeManagementTools`  
>   
> `Restart-Computer`  
>   
> 若要安装数据消除角色服务，键入以下命令，，然后按 ENTER。  
>   
> `Install-WindowsFeature FS-Data-Deduplication -IncludeManagementTools`  
  
### <a name="to-install-file-services-and-the-branchcache-for-network-files-role-service"></a>若要安装文件服务和网络文件角色服务分支缓存  
  
1.  在服务器管理器中，单击**管理**，然后单击**添加角色和功能**。 添加角色和功能向导将打开。 在**在开始之前**，单击**下一步**。  
  
2.  在**选择安装类型**，确保**角色基于或功能的安装**选中，则，然后单击**下一步**。  
  
3.  在**选择目标服务器**，确保正确的服务器选择，然后单击**下一步**。  
  
4.  在**选择服务器角色**中**角色**，请注意，**文件和存储服务**角色是否已安装;单击左侧的角色名称以展开角色服务所选内容旁的箭头，然后单击左侧的箭头**文件和 iSCSI 服务**。  
  
5.  选中复选框**文件服务器**和**网络文件的分支缓存**。  
  
    > [!TIP]  
    > 建议你选择的复选框**数据消除**。
  
    单击**下一步**。  
  
6.  在**选择功能**，单击**下一步**。  
  
7.  在**确认安装选择**，检查你的选择，然后单击**安装**。 **安装进度**窗格显示在安装期间。 安装完成后，单击**关闭**。
