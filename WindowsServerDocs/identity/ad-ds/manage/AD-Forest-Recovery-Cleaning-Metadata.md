---
title: AD 林恢复-清理已删除的 dc 的元数据
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: e7543381-4081-407f-adad-a9de792c6616
ms.technology: identity-adds
ms.openlocfilehash: b71cab51a362a96ab6071e5eed3cf31c4421041c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843038"
---
# <a name="ad-forest-recovery---cleaning-metadata-of-removed-writable-domain-controllers"></a>AD 林恢复-清理已删除的可写域控制器的元数据

>适用于：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

元数据清理将移除标识到复制系统 DC 的 Active Directory 数据。  

使用以下过程适用于你计划添加回网络通过重新安装 AD DS 的 Dc 删除 DC 对象。  
  
如果您使用的版本的 Active Directory 用户和计算机或 Active Directory 站点和服务，包括远程服务器管理工具 (RSAT)，当删除 DC 对象元数据清除是自动执行。  

## <a name="deleting-a-domain-controller-using-active-directory-users-and-computers"></a>删除使用 Active Directory 用户和计算机的域控制器

当您使用 Active Directory 用户和计算机或 Active Directory 管理中心在远程服务器管理工具 (RSAT) 的版本时，元数据清除是自动执行时删除的 DC 对象。 服务器对象和计算机对象也会自动删除。  

或者，您还可用于 Active Directory 站点和服务在 RSAT 中删除的 DC 对象。 如果您使用 Active Directory 站点和服务，则必须删除关联的服务器对象和 NTDS 设置对象，然后才能删除 DC 对象。  

有关安装 RSAT 的信息，请参阅文章[远程服务器管理工具](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools)。
  
下面的过程是相同的域控制器都运行任一 Windows Server 2016、 2012年、 2008 R2 或 2008年。 目标 DC 的元数据清理操作可以运行任何版本的 Windows Server。  
  
### <a name="to-delete-a-domain-controller-object-using-active-directory-users-and-computers-in-rsat"></a>若要删除 RSAT 中使用 Active Directory 用户和计算机的域控制器对象  
  
1. 依次单击 **“开始”**、**“管理工具”** 和 **“Active Directory 用户和计算机”**。  
2. 在控制台树中，双击域容器，然后双击**域控制器**组织单位 (OU)。  
3. 在细节窗格中，右键单击你想要删除，然后单击该 DC**删除**。
   ![删除](media/AD-Forest-Recovery-Cleaning-Metadata/delete1.png) 
4. 单击“是”以确认删除。 选择**此域控制器永久脱机，不再可以使用 Active Directory 域服务安装向导 (DCPROMO) 降级**复选框，然后单击**删除**。  
5. 如果 DC 的全局编录服务器，请单击**是**确认删除。  

## <a name="next-steps"></a>后续步骤

- [AD 林恢复指南](AD-Forest-Recovery-Guide.md)
- [AD 林恢复的过程](AD-Forest-Recovery-Procedures.md)
