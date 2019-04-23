---
title: 请确保虚拟机配置为使用 SR-IOV 时，虚函数驱动程序将正确运行
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: d21e4b93-29bf-423a-a635-71c6d48dc49e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 8d3d0a5008b55d4823cef9a8dd2a7bce4a6a2a33
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852078"
---
# <a name="ensure-that-the-virtual-function-driver-operates-correctly-when-a-virtual-machine-is-configured-to-use-sr-iov"></a>请确保虚拟机配置为使用 SR-IOV 时，虚函数驱动程序将正确运行

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
*虚函数驱动程序运行不正常的一个或多个虚拟机来宾操作系统中。*  
  
## <a name="impact"></a>影响  
*网络性能不是最佳的以下虚拟机上的：*  
  
\<虚拟机的列表 >  
  
## <a name="resolution"></a>分辨率  
*在来宾操作系统，请执行以下操作：验证安装合适的驱动程序以及所有网络设备已启用，并且检查有错误或警告的事件日志。*  
  


