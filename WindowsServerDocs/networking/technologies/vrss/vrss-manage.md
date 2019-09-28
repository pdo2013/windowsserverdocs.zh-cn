---
title: 管理 vRSS
description: 在本主题中，你将使用 Windows PowerShell 命令来管理虚拟机（Vm）和 Hyper-v 主机上的 vRSS。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 0fe5bfc3-591f-4a19-b98a-0668d4c9f93a
ms.localizationpriority: medium
manager: dougkim
ms.date: 09/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9d528f7e658d61f613eedc635fb81d8f18fd59aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405164"
---
# <a name="manage-vrss"></a>管理 vRSS

在本主题中，你将使用 Windows PowerShell 命令来管理虚拟机中的 vRSS \(VMs @ no__t-1 和位于超级 @ no__t-Hqb-2v-fyv 主机上。

>[!NOTE]
>有关本主题中提到的命令的详细信息，请参阅[RSS 和 vRSS 的 Windows PowerShell 命令](vrss-wps.md)。

## <a name="vmq-on-hyper-v-hosts"></a>Hyper-v 主机上的 VMQ

在 Hyper-v 主机上，必须使用控制 VMQ 处理器的关键字。

**查看当前设置：** 

```PowerShell
Get-NetAdapterVmq
```

**配置 VMQ 设置：** 

```PowerShell
Set-NetAdapterVmq
```


## <a name="vrss-on-hyper-v-switch-ports"></a>Hyper-v 交换机端口上的 vRSS

在 Hyper-v 主机上，还必须在超级 @ no__t-0V 虚拟交换机端口上启用 vRSS。

**查看当前设置：**

```PowerShell
Get-VMNetworkAdapter <vm-name> | fl

Get-VMNetworkAdapter -ManagementOS | fl
```
    
以下两个设置都应为**True**。 

- VrssEnabledRequested:True
- VrssEnabled:True
    
>[!IMPORTANT]
>在某些资源限制条件下，超级 @ no__t-0V 虚拟交换机端口可能无法启用此功能。 这是一种暂时性的情况，该功能可能会在后续时间变得可用。
>
>如果**VrssEnabled**为**True**，则为此 VM 或 vNIC 的此超级 @ No__t-hqb-2v-fyv 虚拟交换机端口启用此功能。

**配置交换机端口 vRSS 设置：**

```PowerShell
Set-VMNetworkAdapter <vm-name> -VrssEnabled $TRUE
    
Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
```

## <a name="vrss-in-vms-and-host-vnics"></a>Vm 和主机 Vnic 中的 vRSS

你可以使用用于本机 RSS 的相同命令在 Vm 和主机 Vnic 中配置 vRSS 设置，这也是在主机 Vnic 上启用 RSS 的方式。  

**查看当前设置：**

```PowerShell
Get-NetAdapterRSS
```

**配置 vRSS 设置：**

```PowerShell
Set-NetAdapterRss
```

>[!NOTE]
> 在 VM 中设置配置文件不会影响工作计划。 超级 @ no__t-0V 将做出所有计划决策，并忽略 VM 内的配置文件。

## <a name="disable-vrss"></a>禁用 vRSS

可以禁用 vRSS 来禁用前面提到的任何设置。

- 为物理 NIC 或 VM 禁用 VMQ。

  >[!CAUTION]
  >在物理 NIC 上禁用 VMQ 会严重影响超级 @ no__t 0V 主机处理传入数据包的能力。

- 在超级 @ no__t-1V 主机上的 "超级 @ no__t-0V" 虚拟交换机端口上禁用 VM 的 vRSS。

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled $FALSE
   ```

- 在超级 @ no__t-1V 主机上的 "超级 @ no__t-0V" 虚拟交换机端口上为主机 vNIC 禁用 vRSS。

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $FALSE
   ```

- 禁用 VM 中的 RSS \(or host vNIC @ no__t 中的 VM @no__t-在主机 @ no__t 上的2or

   ```PowerShell
   Disable-NetAdapterRSS *
   ```
