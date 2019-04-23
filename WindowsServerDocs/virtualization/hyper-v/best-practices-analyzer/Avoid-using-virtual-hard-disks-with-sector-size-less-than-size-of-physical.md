---
title: 避免使用虚拟硬盘的扇区大小小于存储虚拟硬盘文件的物理存储的扇区大小
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: b7cf994e-bf4b-4b1b-bad6-ecf7d46d105e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: b6ec2e0995180ecf9ae5986447fdd460a1c7a337
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846258"
---
# <a name="avoid-using-virtual-hard-disks-with-a-sector-size-less-than-the-sector-size-of-the-physical-storage-that-stores-the-virtual-hard-disk-file"></a>避免使用虚拟硬盘的扇区大小小于存储虚拟硬盘文件的物理存储的扇区大小

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统** <br />**System**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。  
  
## <a name="issue"></a>**问题**  
*一个或多个虚拟硬盘的有小于虚拟硬盘文件所在的存储区的物理扇区大小的物理扇区大小。*  
  
## <a name="impact"></a>**影响**  
*使用虚拟硬盘的应用程序的虚拟机上可能会出现性能问题。这会影响以下虚拟机：*  
  
\<虚拟机的列表 >  
  
## <a name="resolution"></a>**解决方法**  
*执行以下操作：*  
  
-   *执行存储迁移将虚拟硬盘移动到新的物理系统*  
  
-   *使用 Windows PowerShell 或 WMI 来启用 VHDX 格式的虚拟硬盘，若要报告的特定扇区大小*  
  
-   *使用注册表设置以启用 VHD 格式虚拟硬盘要报告的物理扇区大小为 4k*  
  


