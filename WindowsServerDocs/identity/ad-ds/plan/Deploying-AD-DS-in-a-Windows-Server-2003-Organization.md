---
ms.assetid: e6b72a80-e8b7-4305-be0c-0a290f468d36
title: 在 Windows Server 2003 组织中部署 AD DS
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 033973ad7a726054f6c47c7154fa54a3767beab4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816358"
---
# <a name="deploying-ad-ds-in-a-windows-server-2003-organization"></a>在 Windows Server 2003 组织中部署 AD DS

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

如果你的组织当前正在运行 Windows Server 2003 Active Directory，则可以通过执行就地升级的部分或所有域控制器的操作系统部署 Windows Server 2008 Active Directory 域服务 (AD DS) Windows Server 2008 或通过引入到您的环境中运行 Windows Server 2008 的域控制器。  
  
可以添加到现有 Windows Server 2003 Active Directory 域中运行 Windows Server 2008 的域控制器之前，必须运行**adprep**，命令行工具。 Adprep 扩展 AD DS 架构、 更新所选对象的默认安全描述符和所需的某些应用程序中添加新的目录对象。 Windows Server 2008 安装盘 (\sources\adprep\adprep.exe) 上提供了 Adprep。 有关详细信息，请参阅 Adprep ([https://go.microsoft.com/fwlink/?LinkId=99215](https://go.microsoft.com/fwlink/?LinkId=99215))。  
  
下图演示了在当前正在运行 Windows Server 2003 Active Directory 的网络环境中部署 Windows Server 2008 AD DS 的步骤。  
  
![在 windows 2003 组织中部署](media/Deploying-AD-DS-in-a-Windows-Server-2003-Organization/900c4eee-1119-4a9a-9310-755597428b71.gif)  
  
> [!NOTE]  
> 如果你想要将域或林功能级别设置为 Windows Server 2008，您的环境中的所有域控制器必须都运行 Windows Server 2008 操作系统。  
  
合并资源域和帐户域，因为林间或林内域重组 Windows Server 2008 AD DS 部署的一部分可能需要从 Windows Server 2003 环境准备好升级。 重组林之间的 AD DS 域可帮助你减少你的组织在 AD DS 中的表示形式的复杂性并帮助降低相关联的管理成本。 重新构建 AD DS 域的林中可帮助你减少你的组织的管理开销，方法是减少复制通信、 减少的用户和组管理员是必需的可通过它，并简化了管理组策略。 有关详细信息，请参阅 ADMT v3.1 迁移指南 ([https://go.microsoft.com/fwlink/?LinkId=93678](https://go.microsoft.com/fwlink/?LinkId=93678))。  
  
有关可用于规划和部署运行 Windows Server 2003 Active Directory 的组织中的 AD DS 的详细任务的列表，请参阅[核对清单：Windows Server 2003 组织中部署 AD DS](https://technet.microsoft.com/library/cc771407.aspx)。  
  


