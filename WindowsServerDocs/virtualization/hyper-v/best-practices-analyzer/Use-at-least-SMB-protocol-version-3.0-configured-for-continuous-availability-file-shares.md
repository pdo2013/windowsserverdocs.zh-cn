---
title: 请至少使用 SMB 协议版本 3.0 配置为持续可用文件的虚拟机存储的文件共享上
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a1fa5cf9-8a48-4f63-bb57-d81e63e77b30
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 67f41293433bd8d14096688fbaa23eb43334c738
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877868"
---
# <a name="use-at-least-smb-protocol-version-30-configured-for-continuous-availability-on-file-shares-that-store-files-for-virtual-machines"></a>请至少使用 SMB 协议版本 3.0 配置为持续可用文件的虚拟机存储的文件共享上

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
*虚拟机文件或虚拟硬盘文件存储在未使用的 SMB 协议版本 3.0 的连续可用性功能配置的网络文件共享上。*  
  
## <a name="impact"></a>**影响**  
*Microsoft 不建议此配置，因为它可能会影响使用服务器虚拟机的可用性。这会影响以下虚拟机：*  
  
\<虚拟机的列表 >  
  
## <a name="resolution"></a>**解决方法**  
执行下列操作之一：  
  
-   将文件移动到配置为持续可用的 SMB 3.0 文件共享。  
  
-   重新配置当前的文件共享，从而提供连续可用性。  
  


