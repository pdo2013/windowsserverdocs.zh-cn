---
ms.assetid: e6b72a80-e8b7-4305-be0c-0a290f468d36
title: 在 Windows Server 2003 组织中部署 AD DS
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 7426ba8fa3ea5077510655ea4877d0e0b69226ff
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408908"
---
# <a name="deploying-ad-ds-in-a-windows-server-2003-organization"></a>在 Windows Server 2003 组织中部署 AD DS

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

如果你的组织当前正在运行 Windows Server 2003 Active Directory，你可以通过将部分或全部域控制器的操作系统就地升级到来部署 Windows Server 2008 Active Directory 域服务（AD DS）。 Windows Server 2008 或引入运行 Windows Server 2008 的域控制器到你的环境中。  
  
在将运行 Windows Server 2008 的域控制器添加到现有 Windows Server 2003 Active Directory 域之前，必须运行**adprep**，这是一个命令行工具。 Adprep 扩展了 AD DS 架构，更新了所选对象的默认安全描述符，并添加了某些应用程序所需的新目录对象。 Windows Server 2008 安装磁盘（\sources\adprep\adprep.exe）上提供了 Adprep。 有关详细信息，请参阅 Adprep （[https://go.microsoft.com/fwlink/?LinkId=99215](https://go.microsoft.com/fwlink/?LinkId=99215)）。  
  
下图显示了在当前运行 Windows Server 2003 Active Directory 的网络环境中部署 Windows Server 2008 AD DS 的步骤。  
  
![在 windows 2003 组织中部署](media/Deploying-AD-DS-in-a-Windows-Server-2003-Organization/900c4eee-1119-4a9a-9310-755597428b71.gif)  
  
> [!NOTE]  
> 如果要将域或林功能级别设置为 Windows Server 2008，环境中的所有域控制器都必须运行 Windows Server 2008 操作系统。  
  
将从 Windows Server 2003 环境升级的资源域和帐户域合并为 Windows Server 2008 AD DS 部署的一部分可能需要林间或林内域重构。 在林之间重新构建 AD DS 域有助于降低组织在 AD DS 中的表示形式的复杂性，并有助于降低关联的管理成本。 重构林中 AD DS 域可降低复制流量，减少所需的用户和组管理的数量，并简化组的管理，从而帮助你降低组织的管理开销政策. 有关详细信息，请参阅 ADMT 3.1 迁移指南（[https://go.microsoft.com/fwlink/?LinkId=93678](https://go.microsoft.com/fwlink/?LinkId=93678)）。  
  
有关可用于在运行 Windows Server 2003 Active Directory 的组织中计划和部署 AD DS 的详细任务的列表，请参阅 [Checklist：在 Windows Server 2003 组织 @ no__t 中部署 AD DS。  
  


