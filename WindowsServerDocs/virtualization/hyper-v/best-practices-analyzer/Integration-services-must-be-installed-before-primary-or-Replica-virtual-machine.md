---
title: 必须先安装 Integration services，然后才能在主虚拟机或副本虚拟机在故障转移后使用备用 IP 地址
description: 此最佳做法分析器规则文本的联机版本，其中包含指向详细信息的链接。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a7fdd185-d6c8-4f58-9b58-2df5827bb056
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 58e744c182fb2013e55e91f58140c6ba14181f9f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393587"
---
# <a name="integration-services-must-be-installed-before-primary-or-replica-virtual-machines-can-use-an-alternate-ip-address-after-a-failover"></a>必须先安装 Integration services，然后才能在主虚拟机或副本虚拟机在故障转移后使用备用 IP 地址

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|Error|  
|**类别**|配置|  
  
在以下部分中，"斜体" 指示在此问题的最佳做法分析器工具中出现的 UI 文本。  
  
## <a name="issue"></a>问题  
*参与复制的虚拟机可以配置为在故障转移时使用特定的 IP 地址，但前提是在虚拟机的来宾操作系统中安装了 integration services。*  
  
## <a name="impact"></a>影响  
@no__t 0In 发生故障转移（计划内、计划外或测试）后，副本虚拟机将使用与主虚拟机相同的 IP 地址联机。此配置可能会导致连接问题。这会影响以下虚拟机： *  
  
@no__t-虚拟机的 0list >  
  
## <a name="resolution"></a>分辨率  
*使用虚拟机连接在虚拟机中安装 integration services。*  
  
从 Windows Server 2016，Windows 虚拟机的 integration services 通过 Windows 更新提供。 确保将这些虚拟机配置为接收 Windows 更新以获取最新版本的 integration services。 Linux 内核现在包含 Linux integration services （.LIS），并已针对新版本进行了更新，但基于较旧内核的 Linux 发行版可能没有最新的增强功能或修补程序。 有关详细信息，请参阅[Windows 上的 Hyper-v 支持的 Linux 和 FreeBSD 虚拟机](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)。


