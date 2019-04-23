---
title: 将 Windows Small Business Server 2011 Standard 迁移到 Windows Server Essentials
description: 介绍如何使用 Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842758"
---
# <a name="migrate-windows-small-business-server-2011-standard-to-windows-server-essentials"></a>将 Windows Small Business Server 2011 Standard 迁移到 Windows Server Essentials

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

本指南介绍了如何将现有 Windows Small Business Server 2011 Standard 域迁移到新硬件上的 Windows Server® 2012 Essentials 以及之后如何迁移设置和数据。 本指南还描述如何完成迁移后从 Windows Server Essentials 网络中删除现有的服务器。  
  
> [!NOTE]
>  若要避免在迁移期间出现问题，Windows Server Essentials 产品开发团队强烈建议在开始迁移之前阅读此文档。  
  
> [!NOTE]

>  若要将你的服务器数据迁移到 Windows Server Essentials 的最新版本，请参阅[迁移到 Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。  

>  若要将你的服务器数据迁移到 Windows Server Essentials 的最新版本，请参阅[迁移到 Windows Server Essentials](../migrate/Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。  

  
## <a name="additional-resources"></a>其他资源  
 有关可帮助指导你完成迁移过程的其他信息、工具和社区资源的链接，请访问 [Windows Small Business Server 迁移](https://go.microsoft.com/fwlink/?LinkId=217520)。  
  
## <a name="terms-and-definitions"></a>术语和定义  
 **源服务器：** 作为迁移设置和数据的来源的现有服务器。  
  
 **目标服务器：** 作为迁移设置和数据的目标的新服务器。  
  
## <a name="migration-process-summary"></a>迁移过程摘要  
 此迁移指南包括下列步骤：  
  

1.  [源服务器以进行 Windows Server Essentials 迁移准备](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md)。  必须确保源服务器和网络已准备好执行迁移操作。 本节指导你完成下列操作：备份源服务器，评估源服务器系统的运行状况，安装最新的 Service Pack 和修补程序，以及验证网络配置。  
  
2.  [在迁移模式下安装 Windows Server Essentials](Install-Windows-Server-Essentials-in-migration-mode.md)。  本部分介绍在迁移模式下在目标服务器上安装 Windows Server Essentials 时应采取的步骤。  
  
3.  [将计算机加入到新的 Windows Server Essentials 服务器](Join-computers-to-the-new-Windows-Server-Essentials-server.md)。  本部分介绍客户端计算机加入新的 Windows Server Essentials 网络以及更新组策略设置。  
  
4.  [将 SBS 2011 设置和数据移到目标服务器](Move-Windows-SBS-2011-Standard-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md)。  本节提供从源服务器迁移数据和设置的相关信息。  
  
5.  [启用 Windows Server Essentials 目标服务器上的文件夹重定向](Enable-folder-redirection-on-the-Windows-Server-Essentials-Destination-Server.md)。  如果在源服务器上启用了文件夹重定向，则可以在目标服务器上也启用同样功能，然后删除旧的“文件夹重定向组策略”设置。  
  
6.  [降级和删除源服务器从新的 Windows Server Essentials 网络](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md)。  从网络中删除源服务器之前，必须强制执行组策略更新并将源服务器降级。  
  
7.  [为 Windows Server Essentials 迁移执行迁移后任务](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md)。  在完成所有设置和数据都迁移到 Windows Server Essentials 后，您可能希望将获得允许的计算机映射到用户帐户。  
  
8.  [运行 Windows Server Essentials 最佳做法分析器](Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md)。  在完成设置和迁移到 Windows Server Essentials 的数据后，应运行 Windows Server Essentials BPA。  

1.  [源服务器以进行 Windows Server Essentials 迁移准备](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md)。  必须确保源服务器和网络已准备好执行迁移操作。 本节指导你完成下列操作：备份源服务器，评估源服务器系统的运行状况，安装最新的 Service Pack 和修补程序，以及验证网络配置。  
  
2.  [在迁移模式下安装 Windows Server Essentials](../migrate/Install-Windows-Server-Essentials-in-migration-mode.md)。  本部分介绍在迁移模式下在目标服务器上安装 Windows Server Essentials 时应采取的步骤。  
  
3.  [将计算机加入到新的 Windows Server Essentials 服务器](../migrate/Join-computers-to-the-new-Windows-Server-Essentials-server.md)。  本部分介绍客户端计算机加入新的 Windows Server Essentials 网络以及更新组策略设置。  
  
4.  [将 SBS 2011 设置和数据移到目标服务器](../migrate/Move-Windows-SBS-2011-Standard-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md)。  本节提供从源服务器迁移数据和设置的相关信息。  
  
5.  [启用 Windows Server Essentials 目标服务器上的文件夹重定向](../migrate/Enable-folder-redirection-on-the-Windows-Server-Essentials-Destination-Server.md)。  如果在源服务器上启用了文件夹重定向，则可以在目标服务器上也启用同样功能，然后删除旧的“文件夹重定向组策略”设置。  
  
6.  [降级和删除源服务器从新的 Windows Server Essentials 网络](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md)。  从网络中删除源服务器之前，必须强制执行组策略更新并将源服务器降级。  
  
7.  [为 Windows Server Essentials 迁移执行迁移后任务](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md)。  在完成所有设置和数据都迁移到 Windows Server Essentials 后，您可能希望将获得允许的计算机映射到用户帐户。  
  
8.  [运行 Windows Server Essentials 最佳做法分析器](../migrate/Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md)。  在完成设置和迁移到 Windows Server Essentials 的数据后，应运行 Windows Server Essentials BPA。  

  
 某些迁移过程需要以管理员身份打开命令提示符窗口。  
  
###  <a name="BKMK_OpenACommandPromptAsAdmin"></a> 若要以管理员身份打开命令提示符窗口，在源服务器上  
  
1.  单击“开始” 。  
  
2.  在搜索框中，键入“cmd”。  
  
3.  在结果列表中，右键单击“cmd”，然后单击“以管理员身份运行”。  
  
#### <a name="to-open-a-command-prompt-window-on-the-destination-server-as-an-administrator"></a>在目标服务器上以管理员身份打开命令提示符窗口的步骤  
  
1.  在“开始”屏幕的搜索框中，键入 **cmd**。  
  
2.  在结果列表中，右键单击“cmd”，然后单击“以管理员身份运行”。
