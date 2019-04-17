---
title: RSS 和 vRSS 的 Windows PowerShell 命令
description: 在本主题中，你将学习如何快速查找有关 Windows PowerShell 命令用于接收方缩放 (RSS) 和虚拟 RSS (vRSS) 的技术参考信息。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 49e93b9f-46d9-4cee-bcda-1c4634893ddd
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/05/2018
ms.openlocfilehash: 10039388009e32c10d71067b835bad65db5607ef
ms.sourcegitcommit: 9ed4c9fe04ebf3ef488170503c9a354c992b6fde
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/03/2018
ms.locfileid: "4339265"
---
# RSS 和 vRSS 的 Windows PowerShell 命令

>适用于：Windows Server（半年频道）、Windows Server 2016

在本主题中，你将学习如何快速查找有关 Windows PowerShell 命令用于接收方缩放 \(RSS\) 和虚拟 RSS \(vRSS\) 的技术参考信息。

使用以下 RSS 命令具有多个处理器或多个核心的物理计算机上配置 RSS。 你可以使用相同的命令的虚拟机上配置 vRSS \(VM\) 运行受支持的操作系统。 有关详细信息，请参阅[Windows PowerShell 中的网络适配器 Cmdlet](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps)。

## 配置 VMQ

vRSS 需要 VMQ 启用和配置。 以下 Windows PowerShell 命令可用于管理 VMQ 设置。

- [禁用 NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/disable-netadaptervmq?view=win10-ps)
- [启用 NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/enable-netadaptervmq?view=win10-ps)
- [获取 NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/get-netadaptervmq?view=win10-ps)
- [设置 NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/set-netadaptervmq?view=win10-ps)

## 启用和配置 RSS 本机主机上

使用以下 PowerShell 命令本机主机上配置 RSS 以及管理 RSS 在 VM 中或在主机虚拟 NIC (vNIC)。 这些命令的参数的一部分也可能会影响在 HYPER-V 主机中的虚拟机队列 \(VMQ\)。  

>[!IMPORTANT]
>启用 RSS 在 VM 中或在主机 vNIC 上是启用和使用 vRSS 的先决条件。

- [禁用 NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/disable-netadapterrss?view=win10-ps)
- [启用 NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/enable-netadapterrss?view=win10-ps)
- [获取 NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/get-netadapterrss?view=win10-ps)
- [设置 NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/Set-NetAdapterRss?view=win10-ps)

## 启用 HYPER-V 虚拟交换机端口上 vRSS

除了启用虚拟机中的 RSS，vRSS 需要启用 HYPER-V 虚拟交换机端口上的 vRSS。 

确定 vRSS 的存在设置和启用或禁用 VM 的功能。

   **查看当前设置：** 

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | fl
   ```

   **启用该功能：**
   
   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled [$True|$False]
   ```

## 启用或禁用在主机 vNIC vRSS

确定 vRSS，存在设置和启用或禁用主机 vNIC 的功能。

   **查看当前设置：** 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | fl
   ```

   **启用或禁用该功能：** 

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled [$True|$False]
   ```

## HYPER-V 虚拟交换机端口上配置的调度模式 
>适用于： Windows Server 2019

在 Windows Server 2019 vRSS 可以更新用于动态处理网络流量的逻辑处理器。  设备支持的驱动程序已启用默认情况下此调度模式。 

确定在系统上，存在的调度模式或修改的 VM 的调度模式。

   **查看当前设置：** 

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | Select 'VRSSQueue'
   ```

   **设置或修改的调度模式：**

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```

## 在主机 vNIC 上配置的调度模式
>适用于： Windows Server 2019

若要确定存在的调度模式或修改主机 vNIC 的调度模式，请使用以下 Windows PowerShell 命令：

   **查看当前设置：** 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | Select 'VRSSQueue'
   ```

   **设置或修改的调度模式：** 

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssQueueSchedulingMode -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```


## 相关主题 
有关详细信息，请参阅下列参考主题。

- [获取 VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmnetworkadapter)
- [设置 VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/set-vmnetworkadapter)

有关详细信息，请参阅[虚拟接收方缩放 (vRSS)](vrss-top.md)。