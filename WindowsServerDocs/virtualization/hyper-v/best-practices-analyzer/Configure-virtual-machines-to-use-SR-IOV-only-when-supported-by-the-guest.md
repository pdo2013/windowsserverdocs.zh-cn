---
title: 虚拟机配置为使用 SR-IOV 仅支持的来宾操作系统时
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 33cf5b68-e43e-47ef-adbc-6b266c1d4dce
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e5c2acb21fe8b11e8f020c6d2ab1742116c23b28
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833358"
---
# <a name="configure-virtual-machines-to-use-sr-iov-only-when-supported-by-the-guest-operating-system"></a>虚拟机配置为使用 SR-IOV 仅支持的来宾操作系统时

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。  
  
## <a name="issue"></a>问题  
*一个或多个虚拟机已配置为使用单根 I/O 虚拟化 (SR-IOV)，但来宾操作系统不支持 SR-IOV*  
  
## <a name="impact"></a>影响  
*SR-IOV 虚拟功能不会分配给以下虚拟机：*  
  
\<虚拟机的列表 >  
  
## <a name="resolution"></a>分辨率  
*所有运行的来宾操作系统不支持 SR-IOV 的虚拟机上禁用 SR-IOV。*  
  
SR-IOV 仅支持某些 64 位 Windows 来宾。 有关详细信息，请参阅[通过生成和来宾的 HYPER-V 功能兼容性](../Hyper-V-feature-compatibility-by-generation-and-guest.md)。  
  


