---
title: "第 7 步：为 Windows Server Essentials 迁移执行后的迁移任务"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d382e3fd-d393-4bd0-883f-db50104a969f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6a61d28f29097bcb6993a471587f4cc1ae0bcc3f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="step-7-perform-post-migration-tasks-for-the-windows-server-essentials-migration"></a>第 7 步：为 Windows Server Essentials 迁移执行后的迁移任务

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

以下任务帮助你完成设置目的地服务器某些相同有源服务器的设置。 你可能已禁用这些设置的一些源服务器上迁移过程中，因此不迁移到目的地服务器。  
  
1.  [删除 DNS 条目源服务器](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
2.  [共享的业务线和应用程序的数据的其他文件夹](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
##  <a name="BKMK_DeleteDNSEntries"></a>删除 DNS 条目源服务器  
 停止使用源代码服务器后，域名服务 (DNS) 服务器仍然可能包含指向源服务器的条目。 删除这些 DNS 条目。  
  
#### <a name="to-delete-dns-entries-that-point-to-the-source-server"></a>若要删除 DNS 条目了指向源服务器  
  
1.  在目标服务器上，打开**DNS 管理**。  
  
2.  在 DNS 管理器中，右键单击服务器名称、单击**属性**，然后单击**转发器**选项卡。  
  
3.  确定指向源服务器转发器列表中是否有一项。 如果没有，请单击**编辑**，然后删除该中的条目**编辑转发器**窗口。  
  
4.  在**DNS 管理**，展开服务器的名称，然后展开**前进查找区域**。  
  
5.  每个前进查找区域中，右键单击该区域中，单击**属性**，然后单击**名称服务器**选项卡。  
  
6.  选择中的某个条目**名称服务器**指向源服务器上，单击的文本框**删除**，然后单击**确定**。  
  
7.  重复步骤 5 和 6，直到删除所有的指针到源代码服务器。  
  
8.  单击**确定**关闭**属性**窗口。  
  
9. 在**DNS 管理**，展开**反向查找区域**。  
  
10. 重复步骤 6 到第 9 步指向源服务器上的所有反向查找区域中都删除。  
  
##  <a name="BKMK_ShareLineOfBusinessAndOtherApplications"></a>共享的业务线和应用程序的数据的其他文件夹  
 必须设置共享的文件夹权限和业务线和其他应用程序的数据文件夹复制到目标服务器 NTFS 权限。 你设置的权限后，将显示共享的文件夹仪表板中上**存储**选项卡。  
  
 如果使用的登录脚本将映射到共享文件夹的驱动器，必须先更新此脚本以将映射到目的地服务器上的驱动器。  
  
## <a name="next-steps"></a>后续步骤  
 对于 Windows Server Essentials 迁移进行迁移后任务。 现在转到[第 8 步-在运行 Windows Server Essentials 最佳实践分析](Step-8--Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md)。  
  

若要查看所有步骤，请参阅[迁移到 Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。

