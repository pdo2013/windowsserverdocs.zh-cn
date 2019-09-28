---
title: 至少使用 SMB 协议版本3.0，配置为用于存储虚拟机文件的文件共享上的连续可用性
description: 此最佳做法分析器规则文本的联机版本。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a1fa5cf9-8a48-4f63-bb57-d81e63e77b30
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 3a6cbb6052e2e50b7fd78792c5e01885d7672932
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393337"
---
# <a name="use-at-least-smb-protocol-version-30-configured-for-continuous-availability-on-file-shares-that-store-files-for-virtual-machines"></a>至少使用 SMB 协议版本3.0，配置为用于存储虚拟机文件的文件共享上的连续可用性

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|  
  
在以下部分中，"斜体" 指示在此问题的最佳做法分析器工具中出现的 UI 文本。  
  
## <a name="issue"></a>**问题**  
*虚拟机文件或虚拟硬盘文件存储在未配置为 SMB 协议版本3.0 的连续可用性功能的网络文件共享上。*  
  
## <a name="impact"></a>**对**  
@no__t 0Microsoft 不推荐此配置，因为这可能会影响使用服务器的虚拟机的可用性。这会影响以下虚拟机： *  
  
@no__t-虚拟机的 0list >  
  
## <a name="resolution"></a>**解决方法**  
执行下列操作之一：  
  
-   将文件移动到配置为持续可用性的 SMB 3.0 文件共享。  
  
-   重新配置当前文件共享以提供连续的可用性。  
  


