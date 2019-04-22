---
title: 解决 vRSS 问题
description: 如果您看不到负载均衡到 VM LPs 流量 vRSS，解决 vRSS 问题。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/04/2018
ms.openlocfilehash: a2d6eb43149361b4270565b63fc99f483f364f74
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824028"
---
## <a name="resolve-vrss-issues"></a>解决 vRSS 问题

如果您已完成所有准备步骤，你仍未看到 vRSS 负载均衡到 VM LPs 流量有不同可能存在的问题。

1. 执行准备步骤之前，vRSS 已被禁用的并且现在必须启用。 你可以运行**Set-vmnetworkadapter**为 VM 启用 vRSS。

   ```PowerShell
   Set-VMNetworkAdapter <VMname> -VrssEnabled $TRUE
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
   ```

2. 在 VM 中或在主机 vNIC 上，已禁用 RSS。 Windows Server 2016 启用了 RSS 默认设置。有人可能已禁用它。 

   - 已启用 = **，则返回 True**

   **查看当前设置：** 

   在 VM 中运行以下 PowerShell cmdlet\(有关在 VM 中的 vRSS\)或在主机上\(为主机 vNIC vRSS\)。

   ```PowerShell
   Get-NetAdapterRss
   ```

   **启用该功能：** 

   若要将值从 False 更改为 True，运行以下 PowerShell cmdlet。

   ```PowerShell
   Enable-NetAdapterRss *
   ```
   
   若要配置 RSS 的另一个系统范围内方法使用 netsh。 将 
   
    ```cmd
   netsh int tcp show global
   ```
   
   若要确保该 RSS 未被全局禁用。 然后根据需要启用它。 此设置不会受到影响的 *-NetAdapterRSS。

3. 如果您发现 VMMQ 未启用配置 vRSS 之后，，验证连接到虚拟交换机，每个适配器上的以下设置：

   - VmmqEnabled = **False**
   - VmmqEnabledRequested = **，则返回 True**

   ![vmmq-enabled](../../media/vmmq-enabled.png)

   **查看当前设置：** 

   ```PowerShell
   Get-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS'
   ```

   **启用该功能：** 

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS' -DisplayValue Enabled”
   ```
 
4. _(Windows Server 2019)_ 不能启用 VMMQ (VmmqEnabled = False) 时设置**VrssQueueSchedulingMode**到**动态**。 一旦启用 VMMQ VrssQueueSchedulingMode 不会更改为动态。<p>**VrssQueueSchedulingMode**的**动态**启用 VMMQ 时需要的驱动程序支持。  VMMQ 是卸载逻辑处理器上的数据包位置，在这种情况下，需要驱动程序支持来利用动态算法。  请安装 NIC 供应商的驱动程序和固件支持动态 VMMQ。



---
