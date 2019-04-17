---
title: "将 Windows 小型企业服务器 2011 年标准迁移到 Windows Server Essentials"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c8325f87-fd79-471b-bf70-3f052692c383
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 5ee0dd816354cb5acc31305de39062e558af1146
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="migrate-windows-small-business-server-2011-standard-to-windows-server-essentials"></a>将 Windows 小型企业服务器 2011 年标准迁移到 Windows Server Essentials

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

本指南介绍了如何为 Windows Server 迁移现有 Windows 小型企业服务器 2011 年标准域® 将软件包 2012 年在新硬件，然后迁移的设置和数据。 此外，本指南介绍了如何后完成迁移的 Windows Server Essentials 网络删除现有的服务器。  
  
> [!NOTE]
>  若要避免在迁移问题，Windows Server Essentials 产品开发团队强烈建议你阅读本文档，然后再开始迁移。  
  
> [!NOTE]

>  若要迁移到最新版本的 Windows Server Essentials 服务器数据，请参阅[迁移到 Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。  

>  若要迁移到最新版本的 Windows Server Essentials 服务器数据，请参阅[迁移到 Windows Server Essentials](../migrate/Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。  

  
## <a name="additional-resources"></a>更多资源  
 工具，和社区资源，以帮助指导你完成迁移的过程的其他信息的链接，请参阅[Windows 小型企业服务器迁移](https://go.microsoft.com/fwlink/?LinkId=217520)。  
  
## <a name="terms-and-definitions"></a>条款和定义  
 **源 Server:**现有服务器从中迁移你的设置和数据。  
  
 **目标 Server:**新服务器正在迁移你的设置和数据。  
  
## <a name="migration-process-summary"></a>迁移进程的摘要  
 此迁移指南包含以下步骤：  
  

1.  [Windows Server Essentials 迁移准备源服务器](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md)。  你必须确保你的源 Server 和网络都准备好进行迁移。 本部分将指导您完成备份源服务器、评估源服务器系统运行状况、安装最新的 service pack 和修复程序，并验证网络配置。  
  
2.  [安装 Windows Server Essentials 迁移型](Install-Windows-Server-Essentials-in-migration-mode.md)。  此部分中介绍了在迁移模式中的目标服务器上安装 Windows Server Essentials 你应执行的步骤。  
  
3.  [计算机到新的 Windows Server Essentials 服务器加入](Join-computers-to-the-new-Windows-Server-Essentials-server.md)。  此部分中介绍如何连接到新的 Windows Server Essentials 网络的客户端计算机和更新组策略设置。  
  
4.  [将 SBS 2011 设置和数据移至目标服务器](Move-Windows-SBS-2011-Standard-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md)。  此部分中提供源服务器有关迁移数据和设置的信息。  
  
5.  [启用 Windows Server Essentials 目标服务器上的文件夹重定向](Enable-folder-redirection-on-the-Windows-Server-Essentials-Destination-Server.md)。  如果源服务器上启用了文件夹重定向，你可以启用目的地服务器上的文件夹重定向和然后再删除旧的文件夹重定向组策略设置。  
  
6.  [降级和从新的 Windows Server Essentials 网络删除源服务器](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md)。  删除从网络来源服务器之前, 你必须强制组策略更新并降级源服务器。  
  
7.  [对 Windows Server Essentials 迁移执行后的迁移任务](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md)。  你完成所有设置和数据都迁移于 Windows Server Essentials 后，你可能想要将允许的计算机与用户帐户。  
  
8.  [运行 Windows Server Essentials 最佳实践分析](Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md)。  你完成迁移的设置和 Windows Server essentials 数据后，你应该运行 Windows Server Essentials BPA。  

1.  [Windows Server Essentials 迁移准备源服务器](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md)。  你必须确保你的源 Server 和网络都准备好进行迁移。 本部分将指导您完成备份源服务器、评估源服务器系统运行状况、安装最新的 service pack 和修复程序，并验证网络配置。  
  
2.  [安装 Windows Server Essentials 迁移型](../migrate/Install-Windows-Server-Essentials-in-migration-mode.md)。  此部分中介绍了在迁移模式中的目标服务器上安装 Windows Server Essentials 你应执行的步骤。  
  
3.  [计算机到新的 Windows Server Essentials 服务器加入](../migrate/Join-computers-to-the-new-Windows-Server-Essentials-server.md)。  此部分中介绍如何连接到新的 Windows Server Essentials 网络的客户端计算机和更新组策略设置。  
  
4.  [将 SBS 2011 设置和数据移至目标服务器](../migrate/Move-Windows-SBS-2011-Standard-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md)。  此部分中提供源服务器有关迁移数据和设置的信息。  
  
5.  [启用 Windows Server Essentials 目标服务器上的文件夹重定向](../migrate/Enable-folder-redirection-on-the-Windows-Server-Essentials-Destination-Server.md)。  如果源服务器上启用了文件夹重定向，你可以启用目的地服务器上的文件夹重定向和然后再删除旧的文件夹重定向组策略设置。  
  
6.  [降级和从新的 Windows Server Essentials 网络删除源服务器](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md)。  删除从网络来源服务器之前, 你必须强制组策略更新并降级源服务器。  
  
7.  [对 Windows Server Essentials 迁移执行后的迁移任务](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md)。  你完成所有设置和数据都迁移于 Windows Server Essentials 后，你可能想要将允许的计算机与用户帐户。  
  
8.  [运行 Windows Server Essentials 最佳实践分析](../migrate/Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md)。  你完成迁移的设置和 Windows Server essentials 数据后，你应该运行 Windows Server Essentials BPA。  

  
 某些迁移过程需要你打开 command prompt 窗口以管理员身份登录。  
  
###  <a name="BKMK_OpenACommandPromptAsAdmin"></a>若要以管理员身份打开源服务器上的 command prompt 窗口  
  
1.  单击**开始**。  
  
2.  在搜索框中，键入**cmd**。  
  
3.  在结果列表中，右键单击**cmd**，然后单击**以管理员身份运行**。  
  
#### <a name="to-open-a-command-prompt-window-on-the-destination-server-as-an-administrator"></a>若要以管理员身份打开目的地服务器上的 command prompt 窗口  
  
1.  在**开始**屏幕上的，在搜索框中，键入**cmd**。  
  
2.  在结果列表中，右键单击**cmd**，然后单击**以管理员身份运行**。
