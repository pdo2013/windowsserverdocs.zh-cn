---
title: 从早期版本迁移到 Windows Server Essentials 或 Windows Server Essentials 体验
description: 介绍如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/20116
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2974fb3a-5150-43fd-a73f-3e5074eb5d03
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 213ee4304d9d4ebdb7580f7f78fdaca78aa454c9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883868"
---
# <a name="migrate-from-previous-versions-to-windows-server-essentials-or-windows-server-essentials-experience"></a>从早期版本迁移到 Windows Server Essentials 或 Windows Server Essentials 体验

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

本指南介绍了如何从 Windows Small Business Server 和 Windows Server Essentials （包括 Windows Server Essentials、 Windows Small Business Server 2011 Standard、 Windows Small Business Server 2011 Essentials、 Windows 的早期版本迁移Small Business Server 2008 和 Windows Small Business Server 2003） 到 Windows Server Essentials 或 Windows Server 2012 R2 安装了 Windows Server Essentials 体验角色。  
  
 **对于具有最多 25 个用户和 50 台设备的环境**，可以按照本指南，以从早期版本的 Windows SBS 迁移到 Windows Server Essentials 中的步骤。  
  
 **对于具有最多 100 个用户和 200 台设备的环境**，可以按照相同的指南将安装了 Windows Server Essentials 体验角色迁移到 Windows Server 2012 R2 Standard 或 Datacenter 版本。  
  
> [!NOTE]
>  为了避免迁移过程中出现问题，Windows Server Essentials 产品开发团队强烈建议你在开始迁移之前先阅读本文档。  
  
## <a name="terms-and-definitions"></a>术语和定义  
 **源服务器** 作为迁移设置和数据的来源的现有服务器。  
  
 **目标服务器** 作为迁移设置和数据的目标的新服务器。  
  
## <a name="migration-process-summary"></a>迁移过程摘要  
 此迁移指南包括下列步骤：  
  
1.  [步骤 1：源服务器以进行 Windows Server Essentials 迁移准备](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md)。  必须确保源服务器和网络已准备好执行迁移操作。 本节指导你完成下列操作：备份源服务器，评估源服务器系统的运行状况，安装最新的 Service Pack 和修补程序，以及验证网络配置。  
  
2.  [步骤 2：Windows Server Essentials 安装为新副本域控制器](Step-2--Install-Windows-Server-Essentials-as-a-new-replica-domain-controller.md)。 本部分介绍如何使用来安装 Windows Server Essentials 或 Windows Server 2012 R2 Standard （已启用 Windows Server Essentials 体验角色） 作为域控制器。  
  
3.  [步骤 3：将计算机加入到新的 Windows Server Essentials 服务器](Step-3--Join-computers-to-the-new-Windows-Server-Essentials-server.md)。  本部分介绍如何将客户端计算机加入到新的服务器运行 Windows Server Essentials 并更新组策略设置。  
  
4.  [步骤 4：将设置和数据移到目标服务器以进行 Windows Server Essentials 迁移](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md)。  本节提供从源服务器迁移数据和设置的相关信息。  
  
5.  [步骤 5：启用在目标服务器以进行 Windows Server Essentials 迁移的文件夹重定向](Step-5--Enable-folder-redirection-on-the-Destination-Server-for-Windows-Server-Essentials-migration.md)。  如果在源服务器上启用了文件夹重定向，则可以在目标服务器上也启用同样功能，然后删除旧的“文件夹重定向组策略”设置。  
  
6.  [步骤 6：降级和删除源服务器从新的 Windows Server Essentials 网络](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md)。  从网络中删除源服务器之前，必须强制执行组策略更新并将源服务器降级。  
  
7.  [步骤 7：为 Windows Server Essentials 迁移执行迁移后任务](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md)。  在完成所有设置和数据都迁移到 Windows Server Essentials 后，您可能希望将获得允许的计算机映射到用户帐户。  
  
8.  [步骤 8：运行 Windows Server Essentials 最佳做法分析器](Step-8--Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md)。  在完成设置和迁移到 Windows Server Essentials 的数据后，应运行 Windows Server Essentials 最佳做法分析器 (BPA)。  
  
 某些迁移过程需要以管理员身份打开命令提示符窗口。 以下过程介绍如何执行此操作。  
  
###  <a name="BKMK_OpenACommandPromptAsAdmin"></a> 若要以管理员身份打开命令提示符窗口，在源服务器上  
  
1.  单击“开始” 。  
  
2.  在搜索框中，键入“cmd”。  
  
3.  在结果列表中，右键单击“cmd”，然后单击“以管理员身份运行”。  
  
#### <a name="to-open-a-command-prompt-window-on-the-destination-server-as-an-administrator"></a>在目标服务器上以管理员身份打开命令提示符窗口的步骤  
  
1.  在“开始”屏幕的搜索框中，键入 **cmd**。  
  
2.  在结果列表中，右键单击“cmd”，然后单击“以管理员身份运行”。  
  
## <a name="see-also"></a>请参阅  
  
-   [将服务器数据迁移到 Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [将服务器数据迁移到 Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

