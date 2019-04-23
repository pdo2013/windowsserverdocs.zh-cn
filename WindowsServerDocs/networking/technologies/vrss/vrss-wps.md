---
title: Windows PowerShell 命令适用于 RSS 和 vRSS
description: 在本主题中，您将学习如何快速查找有关 Windows PowerShell 命令用于接收方缩放 (RSS) 和虚拟 RSS (vRSS) 的技术参考信息。
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833258"
---
# <a name="windows-powershell-commands-for-rss-and-vrss"></a>Windows PowerShell 命令适用于 RSS 和 vRSS

>适用于：Windows 服务器 （半年频道），Windows Server 2016

在本主题中，学习如何快速查找有关 Windows PowerShell 命令用于接收方缩放的技术参考信息\(RSS\)和虚拟 RSS \(vRSS\)。

以下的 RSS 命令用于在具有多个处理器或多个内核的物理计算机上配置 RSS。 可以使用相同的命令的虚拟机上配置 vRSS \(VM\)运行受支持的操作系统。 有关详细信息，请参阅[Windows PowerShell 中的网络适配器 Cmdlet](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps)。

## <a name="configure-vmq"></a>配置 VMQ

vRSS 需要启用并配置 VMQ。 可以使用以下 Windows PowerShell 命令来管理 VMQ 设置。

- [Disable-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/disable-netadaptervmq?view=win10-ps)
- [Enable-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/enable-netadaptervmq?view=win10-ps)
- [Get-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/get-netadaptervmq?view=win10-ps)
- [Set-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/set-netadaptervmq?view=win10-ps)

## <a name="enable-and-configure-rss-on-a-native-host"></a>启用和配置本地主机上的 RSS

使用以下 PowerShell 命令在本地主机上配置 RSS，以及管理 RSS 在 VM 中或在主机上虚拟 NIC (vNIC)。 这些命令的参数的一些可能会影响虚拟机队列\(VMQ\)中的 HYPER-V 主机。  

>[!IMPORTANT]
>启用 RSS 在 VM 中或主机 vNIC 上是启用和使用 vRSS 的先决条件。

- [Disable-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/disable-netadapterrss?view=win10-ps)
- [Enable-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/enable-netadapterrss?view=win10-ps)
- [Get-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/get-netadapterrss?view=win10-ps)
- [Set-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/Set-NetAdapterRss?view=win10-ps)

## <a name="enable-vrss-on-the-hyper-v-virtual-switch-port"></a>启用 vRSS 上超\-V 虚拟交换机端口

除了启用 RSS 在 VM 中的，vRSS 要求启用 vRSS 上超\-V 虚拟交换机端口。 

确定 vRSS 存在设置和启用或禁用该功能的 vm。

   **查看当前设置：** 

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | fl
   ```

   **启用该功能：**
   
   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled [$True|$False]
   ```

## <a name="enable-or-disable-vrss-on-a-host-vnic"></a>启用或禁用主机 vNIC 上 vRSS

确定 vRSS，存在设置和启用或禁用主机 vNIC 的功能。

   **查看当前设置：** 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | fl
   ```

   **启用或禁用该功能：** 

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled [$True|$False]
   ```

## <a name="configure-the-scheduling-mode-on-the-hyper-v-virtual-switch-port"></a>在 HYPER-V 虚拟交换机端口上配置的计划模式 
>适用于：Windows Server 2019

在 Windows Server 2019 vRSS 可以更新用于动态处理网络流量的逻辑处理器。  支持的驱动程序的设备已启用默认情况下此计划模式。 

确定存在计划模式在系统上，或修改计划模式下的 VM。

   **查看当前设置：** 

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | Select 'VRSSQueue'
   ```

   **设置或修改计划模式：**

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```

## <a name="configure-the-scheduling-mode-on-a-host-vnic"></a>在主机 vNIC 上配置计划模式
>适用于：Windows Server 2019

若要确定存在计划模式或修改主机 vNIC 的计划模式，请使用以下 Windows PowerShell 命令：

   **查看当前设置：** 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | Select 'VRSSQueue'
   ```

   **设置或修改计划模式：** 

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssQueueSchedulingMode -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```


## <a name="related-topics"></a>相关主题 
有关详细信息，请参阅以下参考主题。

- [Get-VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmnetworkadapter)
- [Set-VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/set-vmnetworkadapter)

有关详细信息，请参阅[虚拟接收方缩放 (vRSS)](vrss-top.md)。