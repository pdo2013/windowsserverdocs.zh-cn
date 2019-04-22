---
title: 避免使用 VHD 格式差异在生产环境中运行服务器工作负荷的虚拟机上的虚拟硬盘
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 272de33d-2708-4679-8564-ee28848a2839
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: da908d00a6b5c48a61dad89e8c7b08cf80b4314c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819178"
---
# <a name="avoid-using-vhd-format-differencing-virtual-hard-disks-on-virtual-machines-that-run-server-workloads-in-a-production-environment"></a>避免使用 VHD 格式差异在生产环境中运行服务器工作负荷的虚拟机上的虚拟硬盘

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。  
  
## <a name="issue"></a>**问题**  
*一个或多个虚拟机使用差异虚拟硬盘 VHD 格式。*  
  
## <a name="impact"></a>**影响**  
*如果发生电源故障，VHD 格式差异虚拟硬盘可能遇到一致性问题。如果物理磁盘的.vhd 文件中，当发生电源故障时正在修改执行到某个扇区不完整或不正确更新，则可能发生的一致性问题。这会影响以下虚拟机：*  
  
\<虚拟机的列表 >  
  
## <a name="resolution"></a>**解决方法**  
*关闭虚拟机和转换的 VHD 格式差异为 VHDX 格式的虚拟硬盘链或合并到固定虚拟硬盘链。（VHDX 格式具有可靠性机制，可帮助防止电源故障导致损坏的磁盘。）但是，如果附加到早期版本的 Windows 在某些时候可能会不转换虚拟硬盘。Windows 版本低于 Windows Server 2012 不支持 VHDX 格式。*  
  


