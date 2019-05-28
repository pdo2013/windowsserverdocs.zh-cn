---
title: 文件服务器资源管理器 (FSRM) 概述
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: brianlic
ms.technology: storage
ms.topic: article
author: jasongerend
ms.date: 5/14/2018
description: 文件服务器资源管理器 (FSRM) 是一种工具，使您能够管理和分类的 Windows Server 文件服务器上的数据。
ms.openlocfilehash: 8488c7418ac03be53db7164678fad353bc7c637d
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2019
ms.locfileid: "65476128"
---
# <a name="file-server-resource-manager-fsrm-overview"></a>文件服务器资源管理器 (FSRM) 概述

> 适用于：Windows Server 2019，Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2 和 Windows Server （半年频道） 

文件服务器资源管理器 (FSRM) 是 Windows Server 中的一项角色服务，可用于对文件服务器上存储的数据进行管理和分类。 文件服务器资源管理器可用于自动对文件进行分类、 执行任务基于这些分类、 设置文件夹的配额和创建报表监视存储使用情况。

它也是一个小的点，但我们[添加了禁用更改日志](#whats-new)在 Windows Server，1803年的版本。

## <a name="features"></a>功能

文件服务器资源管理器包括以下功能：

-   [配额管理](quota-management.md)允许你限制卷或文件夹中，允许的空间，并且它们可以自动应用于卷创建的新文件夹。 你还可以定义可应用于新卷或文件夹的配额模板。  
-   [文件分类基础结构](classification-management.md)通过分类流程的自动化，以便可以更有效地管理你的数据提供你的数据洞察力。 你可以基于此分类对文件进行分类和应用策略。 示例策略包括限制访问文件的动态访问控制、文件加密和文件过期。 可以使用文件分类规则自动分类文件，也可以修改所选文件或文件夹的属性手动分类文件。
-   [文件管理任务](file-management-tasks.md)可让你基于分类对文件应用有条件的策略或操作。 文件管理任务的条件包括文件位置、分类属性、创建文件的数据、文件的上一次修改日期或上一次访问文件的时间。 文件管理任务可以采取的操作包括对过期文件、加密文件的功能，或运行自定义命令的功能。
-   [文件屏蔽管理](file-screening-management.md)有助于你控制的文件类型用户可存储在文件服务器上。 你可以限制可存储在共享文件上的扩展名。 例如，你可以创建文件屏蔽，不允许包含 MP3 扩展名的文件存储在文件服务器上的个人共享文件夹上。
-   [存储报告](storage-reports-management.md)帮助你确定磁盘使用情况和你的数据分类方式中的趋势。 你还可以监视尝试要保存未授权文件的一组所选用户。  
  
可以配置和管理通过 Windows PowerShell 或通过使用文件服务器资源管理器应用程序包含使用文件服务器资源管理器的功能。
  
> [!IMPORTANT]
>  文件服务器资源管理器支持仅使用 NTFS 文件系统格式化的卷。 不支持弹性文件系统。  
  
## <a name="practical-applications"></a>实际应用程序  
 文件服务器资源管理器的一些实际应用包括：  
  
-   通过动态访问控制方案使用文件分类基础结构，基于在文件服务器上的文件分类方式创建授予文件和文件夹访问权限的策略。  
  
-   创建文件分类规则，将包含至少 10 个 social security number 的任何文件标记为拥有个人可识别信息。  
  
-   使最近 10 年内未修改的文件过期。  
  
-   为每个用户的主目录创建 200 兆字节的配额，并在他们使用 180 兆字节时通知他们。  
  
-   不允许将任何音乐文件存储在个人共享文件夹中。  
  
-   安排将在每个星期日午夜运行的报告，生成自前两天以来最近访问的文件的列表。 这可以帮助确定周末存储活动，并相应地计划服务器停机时间。  

## <a name="whats-new"></a>新增功能-阻止 FSRM 创建更改日志

从 Windows Server 版本 1803，可以从在服务启动时，在卷上创建变更日志 （也称为 USN 日志） 现在阻止文件服务器资源管理器服务。 这可以节省很少的每个卷上的空间，但将禁用实时文件分类。

有关较旧的新功能，请参阅[What's New 文件服务器资源管理器中](https://technet.microsoft.com/library/dn383587.aspx)。

若要防止文件服务器资源管理器创建变更日志部分或全部卷上，在服务启动时，使用以下步骤： 

1. 停止 SRMSVC 服务。 例如，打开 PowerShell 会话中的以管理员身份，然后输入`Stop-Service SrmSvc`。
2. 删除你想要通过使用 fsutil 命令，从而节省空间上的卷 USN 日志： 

      ```
      fsutil usn deletejournal /d <VolumeName>
      ```
    例如：`fsutil usn deletejournal /d c:`

3. 打开注册表编辑器中，例如，通过键入`regedit`在同一个 PowerShell 会话中。
4. 导航到以下项：**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SrmSvc\Settings**
5. （可选） 为跳过更改日志创建适用于整个 server （请跳过此步骤如果你想要仅在特定卷上禁用它）：
    1. 右键单击**设置**键，然后选择**新建** > **DWORD （32 位） 值**。 
    1. 命名值`SkipUSNCreationForSystem`。
    1. 将值设置为**1** （以十六进制）。
6. 若要根据需要跳过的特定卷的变更日志创建：
    1. 获取你想要通过跳过的卷路径`fsutil volume list`命令或以下 PowerShell 命令：
        ```PowerShell
        Get-Volume | Format-Table DriveLetter,FileSystemLabel,Path
        ```
       下面是示例输出：

       ```
        DriveLetter FileSystemLabel Path
        ----------- --------------- ----
                    System Reserved \\?\Volume{8d3c9e8a-0000-0000-0000-100000000000}\
        C                           \\?\Volume{8d3c9e8a-0000-0000-0000-501f00000000}\
       ```
    2. 返回在注册表编辑器中，右键单击**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SrmSvc\Settings**键，然后选择**新建** > **多字符串值**。
    3. 命名值`SkipUSNCreationForVolumes`。
    4. 输入的每个卷上，则跳过创建变更日志，将每个路径放在单独的行上的路径。 例如：

        ```
        \\?\Volume{8d3c9e8a-0000-0000-0000-100000000000}\
        \\?\Volume{8d3c9e8a-0000-0000-0000-501f00000000}\
        ```

        > [!NOTE] 
        > 注册表编辑器可能会告诉您删除空字符串，它显示你可以安全地忽略此警告：*数据类型 REG_MULTI_SZ 不能包含空字符串。注册表编辑器中将删除找到的所有空字符串。*

7. 启动 SRMSVC 服务。 例如，在 PowerShell 会话中输入`Start-Service SrmSvc`。



## <a name="see-also"></a>请参阅

- [动态访问控制](https://technet.microsoft.com/library/dn408191(v=ws.11).aspx) 