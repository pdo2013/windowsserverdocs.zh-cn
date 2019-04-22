---
title: 为 Windows Server Essentials migration1 执行迁移后任务
description: 介绍如何使用 Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821018"
---
# <a name="perform-post-migration-tasks-for-windows-server-essentials-migration1"></a>为 Windows Server Essentials migration1 执行迁移后任务

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

以下任务将帮助你使用一些与源服务器上相同的设置完成目标服务器的设置。 在迁移过程中，你可能已在源服务器上禁用了其中的某些设置，因此它们并未迁移到目标服务器。 或者，它们是你可能要执行的可选配置步骤。  
  

-   [删除源服务器的 DNS 条目](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
-   [共享业务线和其他应用程序数据文件夹](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
-   [在迁移后修复客户端计算机问题](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_FixClientComputerIssuesAfterMigrating)  
  
-   [向内置管理员组提供作为批处理作业登录权限](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_AdminGroup)  

-   [删除源服务器的 DNS 条目](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
-   [共享业务线和其他应用程序数据文件夹](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
-   [在迁移后修复客户端计算机问题](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_FixClientComputerIssuesAfterMigrating)  
  
-   [向内置管理员组提供作为批处理作业登录权限](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_AdminGroup)  

  
##  <a name="BKMK_DeleteDNSEntries"></a> 删除源服务器的 DNS 条目  
 解除对源服务器的授权后，域名服务 (DNS) 服务器可能仍然包含指向源服务器的条目。 删除这些 DNS 条目。  
  
#### <a name="to-delete-dns-entries-that-point-to-the-source-server"></a>删除指向源服务器的 DNS 条目  
  
1.  在目标服务器上，打开“DNS 管理器”。  
  
2.  在 DNS 管理器中，右键单击服务器名称、单击“属性” ，然后单击“转发器”  选项卡。  
  
3.  确定转发器列表中是否存在指向源服务器的条目。 如果存在，请单击“编辑”，然后删除“编辑转发器”窗口中的该条目。  
  
4.  在“DNS 管理器”中，展开服务器名称，然后展开“正向查找区域”。  
  
5.  对于每个正向查找区域，右键单击该区域，单击“属性”，然后单击“名称服务器”选项卡。  
  
6.  在“名称服务器”框中，单击指向源服务器的某个条目，单击“删除”，然后单击“确定”。  
  
7.  请重复步骤 5 和步骤 6，直到删除指向源服务器的所有指针。  
  
8.  单击“确定”以关闭“属性”窗口。  
  
9. 在“DNS 管理器”控制台中，展开“反向查找区域”。  
  
10. 重复步骤 6 到步骤 9，以删除指向源服务器的所有反向查找区域。  
  
##  <a name="BKMK_ShareLineOfBusinessAndOtherApplications"></a> 共享业务线和其他应用程序数据文件夹  
 你必须为已复制到目标服务器的业务线和其他应用程序数据文件夹设置共享文件夹权限和 NTFS 权限。 设置这些权限后，在 Windows Server Essentials 仪表板中显示的共享的文件夹**存储**部分。  
  
 如果要使用登录脚本将驱动器映射到共享文件夹，你必须将该脚本更新为映射到目标服务器上的驱动器。  
  
##  <a name="BKMK_FixClientComputerIssuesAfterMigrating"></a> 在迁移后修复客户端计算机问题  
 如果从 Windows Small Business Server 2003 Premium Edition 的 Microsoft Internet Security 和 Acceleration (ISA) 服务器安装迁移到 Windows Server Essentials，网络上的客户端计算机仍然具有 Microsoft 防火墙客户端和 Internet资源管理器配置为使用代理服务器。  
  
 由于代理服务器不再不存在，这将导致客户端计算机上出现连接性问题。 如果没有配置的不同的代理服务器，客户端计算机将继续使用代理服务器运行 Windows SBS 2003 的服务器。 若要解决此问题，你必须将 Internet Explorer 重新配置为不使用代理服务器或使用新代理服务器。  
  
#### <a name="to-reconfigure-internet-explorer"></a>重新配置 Internet Explorer 的步骤  
  
1.  在 Internet Explorer 中，单击“工具” ，然后单击“Internet 选项” 。  
  
2.  依次单击“连接”选项卡、“LAN 设置”，然后执行以下操作之一：  
  
    -   如果你未在网络上使用代理服务器，请清除“局域网 (LAN) 设置”对话框中的所有复选框。  
  
    -   若要在网络上使用新的代理服务器：  
  
        1.  在“局域网 (LAN) 设置”对话框中，清除“自动配置”部分中的所有复选框。  
  
        2.  在“代理服务器”部分中，验证已选中两个复选框。  
  
        3.  在“地址”框中，键入代理服务器的完全限定域名 (FQDN)。  
  
        4.  在“端口”框中，键入“80”。  
  
3.  单击“确定”两次  。  
  
4.  浏览到网站以确保连接设置正确。  
  
##  <a name="BKMK_AdminGroup"></a> 向内置管理员组提供作为批处理作业登录权限  
 将现有 Windows Small Business Server 2003 域迁移到 Windows Server Essentials 后，您应该向内置管理员组提供作为批处理作业登录权限。 验证内置管理员组仍然具有作为批处理作业登录到目标服务器的权限。 管理员需要此权限以便在目标服务器上无需登录即可运行警报。  
  
#### <a name="to-give-the-built-in-administrators-group-the-right-to-log-on-as-a-batch-job"></a>向内置管理员组提供作为批处理作业登录的权限的步骤  
  
1.  在目标服务器上，打开“组策略管理”管理工具。  
  
2.  在中**组策略管理**控制台树中，展开**林：** *< 服务器名\>*，展开域，，然后展开你的服务器。  
  
3.  展开“域控制器”，右键单击“默认域控制器策略”，然后单击“编辑”。  
  
4.  在中**组策略管理编辑器**，单击**默认域控制器策略 ***< ServerName\>*** 策略**，然后展开**计算机配置**。  
  
5.  依次展开“策略”、“Windows 设置”和“安全设置”。  
  
6.  在“安全设置”树中，展开“本地策略”，然后单击“用户权限分配”。  
  
7.  在结果窗格中，右键单击“作为批处理作业登录”，然后单击“属性”。  
  
8.  在“作为批处理作业属性登录”页面中，单击“添加用户或组”。  
  
9. 在“添加用户或组”对话框中，单击“浏览”。  
  
10. 在“选择用户、计算机或组”对话框中，键入 **Administrators**。  
  
11. 单击“检查名称”以验证内置管理员组是否出现，然后单击“确定”三次以保存该设置。  
  
## <a name="see-also"></a>请参阅  
  

-   [从 Windows SBS 2003 迁移](Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
  
-   [将服务器数据迁移到 Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [从 Windows SBS 2003 迁移](../migrate/Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
  
-   [将服务器数据迁移到 Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

