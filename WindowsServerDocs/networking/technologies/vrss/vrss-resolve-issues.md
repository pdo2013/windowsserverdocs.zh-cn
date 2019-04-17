---
title: 解决 vRSS 问题
description: 如果你看不到 vRSS 加载到 VM Lp 平衡流量，解决 vRSS 问题。
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
ms.sourcegitcommit: 515b4fd5c40fcbae0e12a2c30090384533972353
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2018
ms.locfileid: "8232516"
---
## 解决 vRSS 问题

如果你已完成所有准备步骤，你仍然不会看到 vRSS 加载到 VM Lp 平衡流量，有不同可能出现的问题。

1. 执行准备步骤之前，vRSS 已禁用的并且现在必须启用。 你可以运行**组 VMNetworkAdapter**能够 vRSS 的虚拟机。

   ```PowerShell
   Set-VMNetworkAdapter <VMname> -VrssEnabled $TRUE
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
   ```

2. RSS 已禁用虚拟机中或在主机 vNIC 上。 Windows Server 2016 使 RSS 默认设置。有人可能已禁用它。 

   - 已启用 = **True**

   **查看当前设置：** 

   （适用于在 VM\ vRSS) VM\ 中运行以下 PowerShell cmdlet 或主机上 \ （适用于主机 vNIC vRSS\)。

   ```PowerShell
   Get-NetAdapterRss
   ```

   **启用该功能：** 

   若要从 False 的值更改为 True，运行以下 PowerShell cmdlet。

   ```PowerShell
   Enable-NetAdapterRss *
   ```
   
   配置 RSS 另一个系统级方法使用 netsh。 用途 
   
    ```cmd
   netsh int tcp show global
   ```
   
   若要确保该 RSS 不全局禁用。 并且如有必要启用它。 此设置不会受到影响通过 *-NetAdapterRSS。

3. 如果你发现 VMMQ 未启用配置 vRSS 后，，验证每个附加到虚拟交换机的适配器上的以下设置：

   - VmmqEnabled = **False**
   - VmmqEnabledRequested = **True**

   ![vmmq 启用](../../media/vmmq-enabled.png)

   **查看当前设置：** 

   ```PowerShell
   Get-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS'
   ```

   **启用该功能：** 

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS' -DisplayValue Enabled”
   ```
 
4. _(Windows 2019) Server_你不能启用 VMMQ (VmmqEnabled = False) 为**动态**设置**VrssQueueSchedulingMode**时。 一旦启用 VMMQ VrssQueueSchedulingMode 不会更改为动态。<p>在 VMMQ 处于启用状态时，**动态** **VrssQueueSchedulingMode**需要驱动程序支持。  VMMQ 是数据包放置在逻辑处理器上卸载，因此，需要利用动态算法的驱动程序支持。  请在安装 NIC 供应商的驱动程序和固件支持动态 VMMQ。



---
