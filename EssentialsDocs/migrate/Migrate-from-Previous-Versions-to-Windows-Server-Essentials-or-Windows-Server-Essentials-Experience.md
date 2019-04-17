---
title: "从以前版本迁移到 Windows Server Essentials 或 Windows Server Essentials 体验"
description: "介绍了如何使用 Windows Server Essentials"
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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="migrate-from-previous-versions-to-windows-server-essentials-or-windows-server-essentials-experience"></a>从以前版本迁移到 Windows Server Essentials 或 Windows Server Essentials 体验

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

本指南介绍了如何将迁移从以前版本的 Windows 小型企业服务器和 Windows Server Essentials（包括 Windows Server Essentials、Windows 小型企业服务器 2011 年标准、Windows 小型企业服务器 2011 年软件包、小型企业的 Windows Server 2008 和 Windows 小型企业 Server 2003）Windows Server Essentials 或 Windows Server 2012 R2 安装了 Windows Server Essentials 体验角色。  
  
 **为 25 个最多用户和 50 设备环境**，你可以按照本指南将从以前版本的 Windows SBS 迁移到 Windows Server Essentials 中的步骤。  
  
 **对于达 100 用户和 200 设备环境**，您可以按照相同的指南安装了 Windows Server Essentials 体验角色迁移到 Windows Server 2012 R2 标准或 Datacenter edition。  
  
> [!NOTE]
>  若要避免在迁移问题，Windows Server Essentials 产品开发团队强烈建议你阅读本文档，然后再开始迁移。  
  
## <a name="terms-and-definitions"></a>条款和定义  
 **源服务器**现有服务器从中迁移你的设置和数据。  
  
 **目标服务器**新服务器正在迁移你的设置和数据。  
  
## <a name="migration-process-summary"></a>迁移进程的摘要  
 此迁移指南包含以下步骤：  
  
1.  [第 1 步：准备 Windows Server Essentials 迁移源服务器](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md)。  你必须确保你的源 Server 和网络都准备好进行迁移。 本部分将指导您完成备份源服务器、评估源服务器系统运行状况、安装最新的 service pack 和修复程序，并验证网络配置。  
  
2.  [第 2 步：安装 Windows Server Essentials 为新的副本域控制器](Step-2--Install-Windows-Server-Essentials-as-a-new-replica-domain-controller.md)。 作为域控制器，此部分中介绍了如何安装 Windows Server Essentials 或 Windows Server 2012 R2 Standard（具有 Windows Server Essentials 体验角色已启用）。  
  
3.  [第 3 步：到新的 Windows Server Essentials 服务器加入计算机](Step-3--Join-computers-to-the-new-Windows-Server-Essentials-server.md)。  此部分中介绍了如何加入客户端计算机运行的 Windows Server Essentials 和更新组策略设置新的服务器。  
  
4.  [第 4 步：将设置和数据移动到用于 Windows Server Essentials 迁移的目标服务器](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md)。  此部分中提供源服务器有关迁移数据和设置的信息。  
  
5.  [第 5 步：启用 Windows Server Essentials 迁移目标服务器上的文件夹重定向](Step-5--Enable-folder-redirection-on-the-Destination-Server-for-Windows-Server-Essentials-migration.md)。  如果源服务器上启用了文件夹重定向，你可以启用目的地服务器上的文件夹重定向和然后再删除旧的文件夹重定向组策略设置。  
  
6.  [第 6 步：将降级，从新的 Windows Server Essentials 网络删除源服务器](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md)。  删除从网络来源服务器之前, 你必须强制组策略更新并降级源服务器。  
  
7.  [第 7 步：为 Windows Server Essentials 迁移执行后的迁移任务](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md)。  你完成所有设置和数据都迁移于 Windows Server Essentials 后，你可能想要将允许的计算机与用户帐户。  
  
8.  [第 8 步：运行 Windows Server Essentials 最佳实践分析](Step-8--Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md)。  你完成迁移的设置和 Windows Server essentials 数据后，你应该运行 Windows Server Essentials 最佳做法分析器 (BPA)。  
  
 某些迁移过程需要你打开 Command Prompt 窗口以管理员身份登录。 以下过程介绍如何执行此操作。  
  
###  <a name="BKMK_OpenACommandPromptAsAdmin"></a>若要以管理员身份打开源服务器上的 Command Prompt 窗口  
  
1.  单击**开始**。  
  
2.  在搜索框中，键入**cmd**。  
  
3.  在结果列表中，右键单击**cmd**，然后单击**以管理员身份运行**。  
  
#### <a name="to-open-a-command-prompt-window-on-the-destination-server-as-an-administrator"></a>若要以管理员身份打开目的地服务器上的 Command Prompt 窗口  
  
1.  在**开始**屏幕上的，在搜索框中，键入**cmd**。  
  
2.  在结果列表中，右键单击**cmd**，然后单击**以管理员身份运行**。  
  
## <a name="see-also"></a>请参阅  
  
-   [Windows Server Essentials 到迁移服务器数据](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Windows Server Essentials 到迁移服务器数据](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

