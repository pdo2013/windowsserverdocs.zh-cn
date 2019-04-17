---
title: "对 Windows Server Essentials migration1 执行后的迁移任务"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f2d236a4-0d62-4961-9d1f-332054e06f6d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 535a547ded55cb4afc0942259eadf5222a815274
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="perform-post-migration-tasks-for-windows-server-essentials-migration1"></a>对 Windows Server Essentials migration1 执行后的迁移任务

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

以下任务帮助你完成设置目的地服务器某些相同有源服务器的设置。 你可能已禁用这些设置的一些源服务器上迁移过程中，因此不迁移到目的地服务器。 或者，它们是你可能想要执行的可选配置步骤。  
  

-   [源服务器 DNS 条目中删除](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
-   [共享的业务线和应用程序的数据的其他文件夹](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
-   [修复迁移后计算机问题的客户端](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_FixClientComputerIssuesAfterMigrating)  
  
-   [提供内置的管理员组权作为批量作业登录](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_AdminGroup)  

-   [源服务器 DNS 条目中删除](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
-   [共享的业务线和应用程序的数据的其他文件夹](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
-   [修复迁移后计算机问题的客户端](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_FixClientComputerIssuesAfterMigrating)  
  
-   [提供内置的管理员组权作为批量作业登录](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_AdminGroup)  

  
##  <a name="BKMK_DeleteDNSEntries"></a>源服务器 DNS 条目中删除  
 停止使用源代码服务器后，域名服务 (DNS) 服务器仍然可能包含指向源服务器的条目。 删除这些 DNS 条目。  
  
#### <a name="to-delete-dns-entries-that-point-to-the-source-server"></a>若要删除 DNS 条目了指向源服务器  
  
1.  在目标服务器上，打开**DNS 管理**。  
  
2.  在 DNS 管理器中，右键单击服务器名称、单击**属性**，然后单击**转发器**选项卡。  
  
3.  确定指向源服务器转发器列表中是否有一项。 如果没有，请单击**编辑**，然后删除该中的条目**编辑转发器**窗口。  
  
4.  在**DNS 管理**，展开服务器的名称，然后展开**前进查找区域**。  
  
5.  每个前进查找区域中，右键单击该区域中，单击**属性**，然后单击**名称服务器**选项卡。  
  
6.  单击中的某个条目**名称服务器**指向源服务器上，单击的框**删除**，然后单击**确定**。  
  
7.  重复步骤 5 和 6，直到删除所有的指针到源代码服务器。  
  
8.  单击**确定**关闭**属性**窗口。  
  
9. 在**DNS 管理**控制台中，展开**反向查找区域**。  
  
10. 重复步骤 6 到第 9 步指向源服务器上的所有反向查找区域中都删除。  
  
##  <a name="BKMK_ShareLineOfBusinessAndOtherApplications"></a>共享的业务线和应用程序的数据的其他文件夹  
 必须设置共享的文件夹权限和业务线和其他应用程序的数据文件夹复制到目标服务器 NTFS 权限。 共享的文件夹你设置的权限后，将显示在 Windows Server Essentials 仪表板中**存储**部分。  
  
 如果使用的登录脚本将映射到共享文件夹的驱动器，必须先更新此脚本以将映射到目的地服务器上的驱动器。  
  
##  <a name="BKMK_FixClientComputerIssuesAfterMigrating"></a>修复迁移后计算机问题的客户端  
 如果迁移到 Windows Server Essentials 从 Windows 小型企业服务器 2003 年高级版使用 Microsoft Internet 安全和安装加速 (ISA) 服务器，在网络上的客户端计算机还有 Microsoft 防火墙客户端和 Internet Explorer 配置为使用的代理服务器。  
  
 这会导致连接问题的客户端上的计算机，因为不存在的代理服务器。 如果没有其他的代理服务器配置的客户端计算机继续使用运行 Windows SBS 2003 代理服务器服务器。 若要解决此问题，你必须重新 Internet Explorer 不使用的代理服务器或使用新的代理服务器的配置。  
  
#### <a name="to-reconfigure-internet-explorer"></a>若要重新配置 Internet Explorer  
  
1.  在 Internet Explorer 中，单击**工具**，然后单击**Internet 选项**。  
  
2.  单击**连接**选项卡上，单击**LAN 设置**，然后执行下列情况之一：  
  
    -   如果您没有使用你的网络上的代理服务器，请清除中的复选框**本地网络 (LAN) 设置**对话框。  
  
    -   如果你想要使用新的代理服务器，在你的网络：  
  
        1.  在**本地网络 (LAN) 设置**对话框中的复选框中清除**自动配置**部分。  
  
        2.  在**代理服务器**部分中，验证，这两个复选框处于选中状态。  
  
        3.  在**地址**框中，键入代理服务器的完整的域名 (FQDN)。  
  
        4.  在**端口**框中，键入**80**。  
  
3.  单击**确定**两次。  
  
4.  浏览到的网站，以确保在连接的设置正确。  
  
##  <a name="BKMK_AdminGroup"></a>提供内置的管理员组权作为批量作业登录  
 将现有域 Windows 小型企业 Server 2003 迁移到 Windows Server Essentials 后，你应让内置管理员组作为批量作业登录的权利。 验证内置管理员组仍有权批作业到目标服务器的身份登录。 管理员需要此运行目标服务器上的提醒，无需登录的权限。  
  
#### <a name="to-give-the-built-in-administrators-group-the-right-to-log-on-as-a-batch-job"></a>提供内置管理员进行分组权作为批量作业登录  
  
1.  在目标服务器上，打开**组策略管理**管理工具。  
  
2.  在**组策略管理**控制台树中，展开**森林：***< ServerName\ >*，展开域中，然后展开服务器。  
  
3.  展开**域控制器**，右键单击**默认域控制器策略**，然后单击**编辑**。  
  
4.  在**组策略管理编辑器**，单击**默认域控制器策略***< ServerName\ >***策略**，然后展开**计算机配置**。  
  
5.  展开**策略**，展开**Windows 设置**，然后展开**安全设置**。  
  
6.  在**安全设置**树中，展开**本地策略**，然后单击**用户权限分配**。  
  
7.  在结果窗格中，右键单击**作为批量作业登录**，然后单击属性。  
  
8.  在**登录作为批量作业属性**页上，单击**添加的用户或组**。  
  
9. 在**添加的用户或组**对话框中，单击**浏览**。  
  
10. 在**选择用户、计算机或组**对话框中，键入**管理员**。  
  
11. 单击**检查名称**以验证内置管理员组出现，然后单击**确定**三次以保存设置。  
  
## <a name="see-also"></a>请参阅  
  

-   [从 Windows SBS 2003 迁移](Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
  
-   [Windows Server Essentials 到迁移服务器数据](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [从 Windows SBS 2003 迁移](../migrate/Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
  
-   [Windows Server Essentials 到迁移服务器数据](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

