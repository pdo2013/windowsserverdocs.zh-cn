---
ms.assetid: 7530cafe-98d7-46c9-95d9-e49d39caa021
title: "部署 Windows 2000 组织中的广告 DS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1c46ff6aee03eee4169dbfe0cdff99febad312d2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="deploying-ad-ds-in-a-windows-2000-organization"></a>部署 Windows 2000 组织中的广告 DS

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

如果你的组织当前运行的 Windows 2000 Active Directory，你可以通过执行的部分或全部域控制器的操作系统，对 Windows Server 2008 就地升级或引入到您的环境中运行 Windows Server 2008 域控制器部署 Windows Server 2008 Active Directory 域服务 (广告 DS)。  
  
你可以添加到现有的域的 Active Directory 的 Windows 2000 运行 Windows Server 2008 域控制器之前，你必须运行**adprep**、命令行工具。 Adprep 扩展广告 DS 方案、更新选择的对象的默认安全描述符并根据需要某些应用程序中添加新的目录对象。 Adprep 功能适用于 Windows Server 2008 安装磁盘 (\sources\adprep\adprep.exe)。 有关详细信息，请参阅 Adprep ([https://go.microsoft.com/fwlink/?LinkId=99215](https://go.microsoft.com/fwlink/?LinkId=99215))。  
  
> [!NOTE]  
> 如果你想要执行的现有 Windows 2000 广告 DS 域控制器与 Windows Server 2008 就地升级，必须服务器先升级到 Windows Server 2003，，然后将其升级到 Windows Server 2008。  
  
下图显示的部署 Windows Server 2008 广告 DS 当前运行的 Windows 2000 Active Directory 的网络环境中的步骤。  
  
![在 windows 2000 组织部署](media/Deploying-AD-DS-in-a-Windows-2000-Organization/ee51218a-a858-49d9-8b99-9986679191c1.gif)  
  
> [!NOTE]  
> 如果你想要设置为 Windows Server 2008 的域中，还是森林功能级别，您的环境中的所有域控制器必须都运行 Windows Server 2008 操作系统。  
  
合并为林或重构内联目录林域，可能需要你的 Windows Server 2008 广告 DS 部署的一部分在从 Windows 2000 环境位置升级的资源和帐户的域。 重构林之间的广告 DS 域，可帮助你减少复杂性以及你的组织管理相关的费用。 重构林中广告 DS 域可帮助你针对你的组织管理开销减少减少复制通信、减少的用户和组管理所需，并简化了的组策略管理。 有关详细信息，请参阅 ADMT 3.1 版迁移指南 ([https://go.microsoft.com/fwlink/?LinkId=93678](https://go.microsoft.com/fwlink/?LinkId=93678))。  
  
你可以使用计划，并将其部署广告 DS 当前运行的 Windows 2000 Active Directory 的组织中的详细的任务列表，请参阅[清单：部署 Windows 2000 组织中的广告 DS](https://technet.microsoft.com/library/cc732737.aspx)。  
  


