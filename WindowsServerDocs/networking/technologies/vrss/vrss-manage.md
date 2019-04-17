---
title: 管理 vRSS
description: 在本主题中，你可以使用 Windows PowerShell 命令来管理 vRSS 在虚拟机 (Vm) 和 HYPER-V 主机上。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0fe5bfc3-591f-4a19-b98a-0668d4c9f93a
ms.localizationpriority: medium
manager: dougkim
ms.date: 09/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8af800608bee7037b48141a7a2edb0c872a7aac0
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133833"
---
# 管理 vRSS

在本主题中，你可以使用 Windows PowerShell 命令来管理 vRSS 在虚拟机 \(VMs\) 和 HYPER-V 主机上。

>[!NOTE]
>有关本主题中提到的命令的详细信息，请参阅[RSS 和 vRSS 的 Windows PowerShell 命令](vrss-wps.md)。

## 在 HYPER-V 主机上的 VMQ

HYPER-V 主机上，你必须使用控制 VMQ 处理器的关键字。

**查看当前设置：** 

```PowerShell
Get-NetAdapterVmq
```

**配置 VMQ 设置：** 

```PowerShell
Set-NetAdapterVmq
```


## vRSS 上的 HYPER-V 交换机端口

在 HYPER-V 主机上，你还必须启用 HYPER-V 虚拟交换机端口上的 vRSS。

**查看当前设置：**

```PowerShell
Get-VMNetworkAdapter <vm-name> | fl

Get-VMNetworkAdapter -ManagementOS | fl
```
    
这两个以下设置应为**True**。 

- VrssEnabledRequested: True
- VrssEnabled: True
    
>[!IMPORTANT]
>某些资源限制情况下，在 HYPER-V 虚拟交换机端口可能无法启用此功能。 这是临时的条件，并且该功能可能会在后续时间。
>
>如果**VrssEnabled**为**True**，则此功能将启用该 HYPER-V 虚拟交换机端口-即此虚拟机或 vNIC。

**配置交换机端口 vRSS 设置：**

```PowerShell
Set-VMNetworkAdapter <vm-name> -VrssEnabled $TRUE
    
Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
```

## 在虚拟机和主机 vNICs vRSS

你可以使用相同命令用于本机 RSS 配置 vRSS 设置在虚拟机和主机 vNICs，这也是在主机 vNICs 启用 RSS 的方法。  

**查看当前设置：**

```PowerShell
Get-NetAdapterRSS
```

**配置 vRSS 设置：**

```PowerShell
Set-NetAdapterRss
```

>[!NOTE]
> VM 内的将配置文件设置不会影响调度的工作。 HYPER-V 使所有计划的决策，并忽略 VM 内的配置文件。

## 禁用 vRSS

你可以禁用 vRSS 禁用任何前面提到的设置。

- 为物理 NIC 或虚拟机禁用 VMQ。

  >[!CAUTION]
  >在物理上禁用 VMQ NIC 严重影响 HYPER-V 主机的处理传入数据包的能力。

- 在 HYPER-V 主机上的 HYPER-V 虚拟交换机端口上的虚拟机禁用 vRSS。

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled $FALSE
   ```

- 禁用 HYPER-V 主机上的 HYPER-V 虚拟交换机端口上主机 vNIC 的 vRSS。

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $FALSE
   ```

- 禁用虚拟机中的 RSS \(or host vNIC\) VM 内 \ （或 host\）

   ```PowerShell
   Disable-NetAdapterRSS *
   ```
