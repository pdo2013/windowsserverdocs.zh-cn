---
title: "文件服务器资源管理器 (FSRM) 概述"
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: brianlic
ms.technology: storage
ms.topic: article
author: jasongerend
ms.date: 7/7/2017
description: "文件服务器资源管理器 (FSRM) 是一个可用于对 Windows Server 文件服务器上的数据进行管理和分类的工具。"
ms.openlocfilehash: ddfc0a0f4bede89a3c3a624d4f128717d0f1ac08
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="file-server-resource-manager-fsrm-overview"></a>文件服务器资源管理器 (FSRM) 概述

> 适用于：Windows Server（半年频道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

文件服务器资源管理器 (FSRM) 是 Windows Server 中的一项角色服务，可用于对文件服务器上存储的数据进行管理和分类。 文件服务器资源管理器包括以下功能：
  
-   **文件分类基础结构**：文件分类基础结构通过分类流程的自动化提供对数据的洞察力，从而能够更有效地管理数据。 你可以基于此分类对文件进行分类和应用策略。 示例策略包括限制访问文件的动态访问控制、文件加密和文件过期。 可以使用文件分类规则自动分类文件，也可以修改所选文件或文件夹的属性手动分类文件。  
  
-   **文件管理任务**：文件管理任务可用于基于分类对文件应用有条件的策略或操作。 文件管理任务的条件包括文件位置、分类属性、创建文件的数据、文件的上一次修改日期或上一次访问文件的时间。 文件管理任务可以采取的操作包括对过期文件、加密文件的功能，或运行自定义命令的功能。  
  
-   **配额管理**：借助配额可限制卷或文件夹可拥有的空间，并且它们可自动应用于卷上创建的新文件夹。 此外，可以定义可应用于新卷或文件夹的配额模板。  
  
-   **文件屏蔽管理**：文件屏蔽可帮助控制用户可存储在文件服务器上的文件类型。 你可以限制可存储在共享文件上的扩展名。 例如，你可以创建文件屏蔽，不允许包含 MP3 扩展名的文件存储在文件服务器上的个人共享文件夹上。  
  
-   **存储报告**：存储报告有助于确定磁盘使用的趋势以及数据分类的方式。 还可以用于监视尝试要保存未授权文件的一组所选用户。  
  
 通过使用文件服务器资源管理器 Microsoft 管理控制台 (MMC) 或使用 Windows PowerShell，可以配置和管理文件服务器资源管理器包含的功能。  
  
> [!IMPORTANT]
>  文件服务器资源管理器支持仅使用 NTFS 文件系统格式化的卷。 不支持弹性文件系统。  
  
## <a name="practical-applications"></a>实际应用程序  
 文件服务器资源管理器的一些实际应用程序包括：  
  
-   通过动态访问控制方案使用文件分类基础结构，基于在文件服务器上的文件分类方式创建授予文件和文件夹访问权限的策略。  
  
-   创建文件分类规则，将包含至少 10 个社会安全保险号的任何文件标记为拥有个人可识别信息。  
  
-   使最近 10 年内未修改的文件过期。  
  
-   为每个用户的主目录创建 200 兆字节的配额，并在他们使用 180 兆字节时通知他们。  
  
-   不允许将任何音乐文件存储在个人共享文件夹中。  
  
-   安排将在每个星期天午夜运行的报告，生成自前两天以来最近访问的文件的列表。 这可以帮助确定周末存储活动，并相应地计划服务器停机时间。  

## <a name="see-also"></a>另请参阅

- [文件服务器资源管理器中的新功能](https://technet.microsoft.com/library/dn383587.aspx)
- [动态访问控制](https://technet.microsoft.com/library/dn408191(v=ws.11).aspx) 