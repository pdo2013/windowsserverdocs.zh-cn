---
title: 主数据库之前，必须安装 integration services 或副本虚拟机可以在故障转移后使用备用的 IP 地址
description: 此最佳实践分析程序规则，并详细信息的链接的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a7fdd185-d6c8-4f58-9b58-2df5827bb056
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 1ff8dbfd71655aee86ba7d0feac87ec2267a2171
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865508"
---
# <a name="integration-services-must-be-installed-before-primary-or-replica-virtual-machines-can-use-an-alternate-ip-address-after-a-failover"></a>主数据库之前，必须安装 integration services 或副本虚拟机可以在故障转移后使用备用的 IP 地址

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|错误|  
|**类别**|配置|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。  
  
## <a name="issue"></a>问题  
*参与复制的虚拟机可以配置为使用特定的 IP 地址发生故障转移，但只有当虚拟机的来宾操作系统中安装集成服务。*  
  
## <a name="impact"></a>影响  
*在故障转移 （计划、 未计划，或测试） 的情况下，副本虚拟机将进入联机状态与主虚拟机中使用相同的 IP 地址。此配置可能会导致连接问题。这会影响以下虚拟机：*  
  
\<虚拟机的列表 >  
  
## <a name="resolution"></a>分辨率  
*使用虚拟机连接在虚拟机中安装集成服务。*  
  
截至 Windows Server 2016，适用于 Windows 虚拟机集成服务是通过 Windows 更新传递的。 请确保这些虚拟机配置为接收 Windows 更新以获取最新版本的 integration services。 Linux 内核现在包含 Linux integration services (LIS)，并更新为新版本中，但 Linux 分发版基于较旧的内核可能没有最新的增强功能或修补程序。 有关详细信息，请参阅[Windows 上的 HYPER-V 的支持的 Linux 和 FreeBSD 虚拟机](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)。


