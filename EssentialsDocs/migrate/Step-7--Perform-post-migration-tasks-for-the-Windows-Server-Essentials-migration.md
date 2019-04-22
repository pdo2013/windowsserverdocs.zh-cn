---
title: 步骤 7：为 Windows Server Essentials 迁移执行迁移后任务
description: 介绍如何使用 Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819158"
---
# <a name="step-7-perform-post-migration-tasks-for-the-windows-server-essentials-migration"></a>步骤 7：为 Windows Server Essentials 迁移执行迁移后任务

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

以下任务将帮助你使用一些与源服务器上相同的设置完成目标服务器的设置。 在迁移过程中，你可能已在源服务器上禁用了其中的某些设置，因此它们并未迁移到目标服务器。  
  
1.  [删除源服务器的 DNS 条目](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
2.  [共享业务线和其他应用程序数据文件夹](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
##  <a name="BKMK_DeleteDNSEntries"></a> 删除源服务器的 DNS 条目  
 解除对源服务器的授权后，域名服务 (DNS) 服务器可能仍然包含指向源服务器的条目。 删除这些 DNS 条目。  
  
#### <a name="to-delete-dns-entries-that-point-to-the-source-server"></a>删除指向源服务器的 DNS 条目  
  
1.  在目标服务器上，打开“DNS 管理器”。  
  
2.  在 DNS 管理器中，右键单击服务器名称、单击“属性” ，然后单击“转发器”  选项卡。  
  
3.  确定转发器列表中是否存在指向源服务器的条目。 如果存在，请单击“编辑”，然后删除“编辑转发器”窗口中的该条目。  
  
4.  在“DNS 管理器”中，展开服务器名称，然后展开“正向查找区域”。  
  
5.  对于每个正向查找区域，右键单击该区域，单击“属性”，然后单击“名称服务器”选项卡。  
  
6.  在“名称服务器”文本框中，选中指向源服务器的某个条目、单击“删除”，然后单击“确定”。  
  
7.  请重复步骤 5 和步骤 6，直到删除指向源服务器的所有指针。  
  
8.  单击“确定”以关闭“属性”窗口。  
  
9. 在“DNS 管理器” 中，展开“反向查找区域” 。  
  
10. 重复步骤 6 到步骤 9，以删除指向源服务器的所有反向查找区域。  
  
##  <a name="BKMK_ShareLineOfBusinessAndOtherApplications"></a> 共享业务线和其他应用程序数据文件夹  
 你必须为已复制到目标服务器的业务线和其他应用程序数据文件夹设置共享文件夹权限和 NTFS 权限。 设置这些权限后，共享文件夹将显示在“存储”  选项卡上的仪表板中。  
  
 如果要使用登录脚本将驱动器映射到共享文件夹，你必须将该脚本更新为映射到目标服务器上的驱动器。  
  
## <a name="next-steps"></a>后续步骤  
 你为 Windows Server Essentials 迁移执行迁移后任务。 现在，转到[步骤 8-运行 Windows Server Essentials 最佳做法分析器](Step-8--Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md)。  
  

若要查看所有步骤，请参阅[迁移到 Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。

