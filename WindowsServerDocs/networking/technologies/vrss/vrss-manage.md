---
title: 管理 vRSS
description: 在本主题中，您可以使用 Windows PowerShell 命令来管理 vRSS 在虚拟机 (Vm) 和 HYPER-V 主机上。
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856188"
---
# <a name="manage-vrss"></a>管理 vRSS

在本主题中，您使用 Windows PowerShell 命令来管理虚拟机中的 vRSS \(Vm\)在超\-V 主机。

>[!NOTE]
>有关本主题中提到的命令的详细信息，请参阅[Windows PowerShell 命令适用于 RSS 和 vRSS](vrss-wps.md)。

## <a name="vmq-on-hyper-v-hosts"></a>VMQ 的 HYPER-V 主机上

在 HYPER-V 主机，必须使用控制 VMQ 处理器的关键字。

**查看当前设置：** 

```PowerShell
Get-NetAdapterVmq
```

**配置 VMQ 设置：** 

```PowerShell
Set-NetAdapterVmq
```


## <a name="vrss-on-hyper-v-switch-ports"></a>vRSS 上的 HYPER-V 交换机端口

在 HYPER-V 主机上，您还必须启用 vRSS 上超\-V 虚拟交换机端口。

**查看当前设置：**

```PowerShell
Get-VMNetworkAdapter <vm-name> | fl

Get-VMNetworkAdapter -ManagementOS | fl
```
    
这两个以下的设置应该 **，则返回 True**。 

- VrssEnabledRequested:True
- VrssEnabled:True
    
>[!IMPORTANT]
>在某些资源限制情况下 Hyper\-V 虚拟交换机端口可能导致无法启用此功能。 这是一种临时情况，并且该功能可能在后续时可用。
>
>如果**VrssEnabled**是**True**，然后该功能可用于此超\-V 虚拟交换机端口 — 也就是说，此 VM 或 vNIC。

**配置交换机端口 vRSS 设置：**

```PowerShell
Set-VMNetworkAdapter <vm-name> -VrssEnabled $TRUE
    
Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
```

## <a name="vrss-in-vms-and-host-vnics"></a>在虚拟机和主机 Vnic vRSS

可以使用相同的命令用于本机 RSS 的也是要在主机 Vnic 上启用 RSS 的方法中虚拟机和主机 Vnic 配置 vRSS 设置。  

**查看当前设置：**

```PowerShell
Get-NetAdapterRSS
```

**配置 vRSS 设置：**

```PowerShell
Set-NetAdapterRss
```

>[!NOTE]
> 设置 VM 内的配置文件不会影响工作计划。 超\-V 使所有计划决策，并忽略内部 VM 的配置文件。

## <a name="disable-vrss"></a>禁用 vRSS

您可以禁用 vRSS 若要禁用的任何前面提到的设置。

- 禁用 VMQ 的物理 NIC 或 VM。

  >[!CAUTION]
  >在物理上禁用 VMQ NIC 会严重影响的能力的 Hyper\-主机来处理传入的数据包。

- 对超上的虚拟机禁用 vRSS\-Hyper V 虚拟交换机端口\-V 主机。

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled $FALSE
   ```

- 禁用主机 vNIC 上超的 vRSS\-Hyper V 虚拟交换机端口\-V 主机。

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $FALSE
   ```

- 禁用虚拟机中的 RSS\(或主机 vNIC\)在 VM 内部\(或在主机上\)

   ```PowerShell
   Disable-NetAdapterRSS *
   ```
