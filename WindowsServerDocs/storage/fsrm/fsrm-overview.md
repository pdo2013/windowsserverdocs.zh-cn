---
title: 文件服务器资源管理器 (FSRM) 概述
ms.prod: windows-server
ms.author: jgerend
ms.manager: brianlic
ms.technology: storage
ms.topic: article
author: jasongerend
ms.date: 5/14/2018
description: 文件服务器资源管理器（FSRM）是一个工具，可用于管理和分类 Windows Server 文件服务器上的数据。
ms.openlocfilehash: 719176307afc320ad676fd1acfc07ad9d15920cf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394165"
---
# <a name="file-server-resource-manager-fsrm-overview"></a>文件服务器资源管理器 (FSRM) 概述

> 适用于：Windows Server 2019，Windows Server 2016，Windows Server 2012 R2，Windows Server 2012，Windows Server 2008 R2，Windows Server （半年频道） 

文件服务器资源管理器 (FSRM) 是 Windows Server 中的一项角色服务，可用于对文件服务器上存储的数据进行管理和分类。 您可以使用文件服务器资源管理器自动对文件进行分类，基于这些分类执行任务，设置文件夹配额，以及创建报告监视存储使用情况。

这是一个小点，但我们也添加了在 Windows Server 版本1803中[禁用更改日志的功能](#whats-new)。

## <a name="features"></a>功能

文件服务器资源管理器包括以下功能：

-   [配额管理](quota-management.md)允许你限制卷或文件夹允许的空间，并且它们可自动应用于卷上创建的新文件夹。 你还可以定义可应用于新卷或文件夹的配额模板。  
-   [文件分类基础结构](classification-management.md)通过自动化分类流程来提供对你的数据的见解，以便你可以更有效地管理数据。 你可以基于此分类对文件进行分类和应用策略。 示例策略包括限制访问文件的动态访问控制、文件加密和文件过期。 可以使用文件分类规则自动分类文件，也可以修改所选文件或文件夹的属性手动分类文件。
-   [文件管理任务](file-management-tasks.md)使你可以基于文件的分类对文件应用条件策略或操作。 文件管理任务的条件包括文件位置、分类属性、创建文件的数据、文件的上一次修改日期或上一次访问文件的时间。 文件管理任务可以采取的操作包括对过期文件、加密文件的功能，或运行自定义命令的功能。
-   [文件屏蔽管理](file-screening-management.md)可帮助控制用户可存储在文件服务器上的文件类型。 你可以限制可存储在共享文件上的扩展名。 例如，你可以创建文件屏蔽，不允许包含 MP3 扩展名的文件存储在文件服务器上的个人共享文件夹上。
-   [存储报告](storage-reports-management.md)可帮助你确定磁盘使用的趋势以及数据的分类方式。 你还可以监视尝试要保存未授权文件的一组所选用户。  
  
文件服务器资源管理器附带的功能可以通过使用文件服务器资源管理器应用程序或使用 Windows PowerShell 来配置和管理。
  
> [!IMPORTANT]
>  文件服务器资源管理器支持仅使用 NTFS 文件系统格式化的卷。 不支持弹性文件系统。  
  
## <a name="practical-applications"></a>实际应用程序  
 文件服务器资源管理器的一些实际应用包括：  
  
-   通过动态访问控制方案使用文件分类基础结构，基于在文件服务器上的文件分类方式创建授予文件和文件夹访问权限的策略。  
  
-   创建文件分类规则，将包含至少 10 个 social security number 的任何文件标记为拥有个人可识别信息。  
  
-   使最近 10 年内未修改的文件过期。  
  
-   为每个用户的主目录创建 200 mb 的配额，并在使用 180 mb 时通知他们。  
  
-   不允许将任何音乐文件存储在个人共享文件夹中。  
  
-   安排将在每个星期日午夜运行的报告，生成自前两天以来最近访问的文件的列表。 这可以帮助确定周末存储活动，并相应地计划服务器停机时间。  

## <a name="whats-new"></a>新增功能-阻止 FSRM 创建变更日志

从 Windows Server 版本1803开始，现在可以阻止文件服务器资源管理器服务在服务启动时在卷上创建更改日志（也称为 USN 日志）。 这可以在每个卷上保留少许空间，但会禁用实时文件分类。

有关较旧的新功能，请参阅[文件服务器资源管理器中的新增](https://technet.microsoft.com/library/dn383587.aspx)功能。

若要防止文件服务器在服务启动时资源管理器在部分或全部卷上创建更改日志，请使用以下步骤： 

1. 停止 SRMSVC 服务。 例如，以管理员身份打开 PowerShell 会话，然后输入`Stop-Service SrmSvc`。
2. 使用 fsutil 命令删除要在其中节省空间的卷的 USN 日志： 

      ```
      fsutil usn deletejournal /d <VolumeName>
      ```
    例如： `fsutil usn deletejournal /d c:`

3. 打开注册表编辑器，例如，在同一个`regedit` PowerShell 会话中键入。
4. 导航到以下注册表项：**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SrmSvc\Settings**
5. 若要根据需要跳过为整个服务器创建更改日志，请跳过此步骤（如果只想在特定卷上禁用此步骤）：
    1. 右键单击 "**设置**" 项，然后选择 "**新建** > **DWORD （32位）" 值**。 
    1. 将值`SkipUSNCreationForSystem`命名为。
    1. 将值设置为**1** （十六进制）。
6. 如果需要，可以跳过针对特定卷创建更改日志：
    1. 使用`fsutil volume list`命令或以下 PowerShell 命令获取要跳过的卷路径：
        ```PowerShell
        Get-Volume | Format-Table DriveLetter,FileSystemLabel,Path
        ```
       下面是一个示例输出：

       ```
        DriveLetter FileSystemLabel Path
        ----------- --------------- ----
                    System Reserved \\?\Volume{8d3c9e8a-0000-0000-0000-100000000000}\
        C                           \\?\Volume{8d3c9e8a-0000-0000-0000-501f00000000}\
       ```
    2. 返回到注册表编辑器，右键单击**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SrmSvc\Settings**项，然后选择 "**新建** > **多字符串值**"。
    3. 将值`SkipUSNCreationForVolumes`命名为。
    4. 输入在其上跳过创建变更日志的每个卷的路径，将每个路径置于单独的行上。 例如：

        ```
        \\?\Volume{8d3c9e8a-0000-0000-0000-100000000000}\
        \\?\Volume{8d3c9e8a-0000-0000-0000-501f00000000}\
        ```

        > [!NOTE] 
        > 注册表编辑器可能会告诉您它删除了空字符串，并显示此警告，您可以放心地忽略此警告：类型为 REG_MULTI_SZ 的 @no__t 0Data 不能包含空字符串。注册表编辑器将删除所有找到的空字符串。*

7. 启动 SRMSVC 服务。 例如，在 PowerShell 会话中输入`Start-Service SrmSvc`。



## <a name="see-also"></a>请参阅

- [动态访问控制](https://technet.microsoft.com/library/dn408191(v=ws.11).aspx) 