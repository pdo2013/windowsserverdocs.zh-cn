---
title: "解决 Windows Server Essentials 中的文件历史记录"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed062945-27e9-4572-b1bb-6c8cf1b9c2f4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 34442565b54b089064c1fa19317a24f591e44fda
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="troubleshoot-file-history-in-windows-server-essentials"></a>解决 Windows Server Essentials 中的文件历史记录

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials 
  
## <a name="troubleshoot-issues-with-user-file-history-backups"></a>用户的文件历史记录备份问题疑难解答  
 管理的文件历史记录备份用户或计算机已添加到运行 Windows Server Essentials 的服务器时可能会出现以下的问题。  
  
### <a name="file-history-data-is-not-automatically-deleted"></a>文件历史记录的数据不会自动删除  
 如果，可能无法获取自动删除的文件历史记录数据：  
  
-   在删除的用户帐户时，你选择删除用户帐户的 s 文件历史记录数据，并选择要删除的数据手动。  
  
-   当你尝试要删除的文件历史记录数据时的文件历史记录数据已由其他进程使用。  
  
 若要解决此问题，则必须手动删除使用下面的过程中的文件历史记录：  
  
####  <a name="BKMK_manuallyDelete"></a>若要手动删除用户或计算机的文件历史记录备份  
  
1.  以管理员身份登录到服务器上。  
  
2.  以管理员身份运行文件资源管理器。  
  
3.  导航到文件历史记录备份文件夹。 默认位置是 C:\ServerFolders\File 历史记录备份。  
  
4.  删除存储文件历史记录备份的共享的文件夹：  
  
    -   若要删除的用户的文件历史记录，请删除具有用户名 s 的文件历史记录备份子文件夹。  
  
    -   若要删除一台计算机的文件历史记录，删除的文件历史记录备份子文件夹具有计算机名称。 例如，如果用户停用的 < MyComputer01\ > 她开始 < MyComputer02\ >，她新笔记本电脑上的工作后会删除 C:\ServerFolders\File 历史记录 Backups\\ < MyAccount\ > \\ < MyComputer01\ > 后你确认了与用户她已将所有文件和文件夹都转移到她新笔记本电脑，并且在将来具有不需要的文件历史记录。  
  
### <a name="cannot-apply-file-history-setting-to-a-new-user"></a>无法将文件历史记录设置应用到新的用户  
 如果你添加新的用户其用户名等同于已从 Windows Server Essentials 用户的用户名，，Windows Server Essentials 尝试创建一个文件夹存储新用户的文件历史记录时，可能会由于命名冲突失败新的用户的文件历史记录配置。 若要解决此问题，可以重命名已删除的用户的文件历史记录文件夹。  
  
##### <a name="to-locate-user-file-history-on-the-server"></a>若要找到服务器上的用户文件历史记录  
  
1.  以管理员身份登录到服务器上。  
  
2.  在 Windows Server Essentials 仪表板中，单击**存储**。  
  
3.  在**服务器文件夹**选项卡、记下的文件历史记录备份文件夹的位置。 默认位置是 %SystemDrive%\ServerFolders\File Backups\\ 历史记录。  
  
##### <a name="to-resolve-file-history-issues-for-a-new-user-with-a-name-conflict"></a>若要解决为新用户名称冲突使用文件历史记录问题  
  
1.  以管理员身份登录到服务器上。  
  
2.  以管理员身份运行文件资源管理器。  
  
3.  导航到文件历史记录备份文件夹。 默认位置是 C:\ServerFolders\File 历史记录备份。  
  
     文件历史记录备份文件夹有一个已添加到 Windows Server Essentials 每个用户帐户的子文件夹。 例如，用户三张文件历史记录会子文件夹中的文件历史记录 Backups\JohnSmith 存储。  
  
4.  例如，删除对用户的子文件夹重命名**<*用户名*> _Deleted **。 如果你不再需要用户的文件历史记录时，你可以删除文件夹。  
  

5.  现在，你可以添加新的用户。 有关说明，请参阅添加的用户帐户？在[管理用户帐户](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md)。  
  
### <a name="a-user-account-was-removed-but-the-users-file-history-remains"></a>用户帐户已删除，但仍然用户的文件历史记录  
 在某些情况下网络管理员可能会选择用户或计算机卸下服务器，而是保留文件历史记录备份供以后使用。 当你不再需要的文件历史记录时，删除服务器上的共享文件夹用户或计算机的文件历史记录备份文件夹。 若要执行此操作，请参阅[手动删除用户或计算机的文件历史记录备份](Troubleshoot-File-History-in-Windows-Server-Essentials.md#BKMK_manuallyDelete)。  

5.  现在，你可以添加新的用户。 有关说明，请参阅添加的用户帐户？在[管理用户帐户](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md)。  
  
### <a name="a-user-account-was-removed-but-the-users-file-history-remains"></a>用户帐户已删除，但仍然用户的文件历史记录  
 在某些情况下网络管理员可能会选择用户或计算机卸下服务器，而是保留文件历史记录备份供以后使用。 当你不再需要的文件历史记录时，删除服务器上的共享文件夹用户或计算机的文件历史记录备份文件夹。 若要执行此操作，请参阅[手动删除用户或计算机的文件历史记录备份](../support/Troubleshoot-File-History-in-Windows-Server-Essentials.md#BKMK_manuallyDelete)。  

  
## <a name="see-also"></a>请参阅  
  
-   [管理客户端备份](../manage/Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md)  
  

-   [支持 Windows Server Essentials](Support-Windows-Server-Essentials.md)

-   [支持 Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)

