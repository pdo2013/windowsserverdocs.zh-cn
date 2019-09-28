---
title: NIC 组合 MAC 地址的使用和管理
description: 当你使用 "交换机独立" 模式配置 NIC 组，并使用 "地址哈希" 或 "动态负载" 分发时，团队将在出站流量上使用主要 NIC 组成员的媒体访问控制（MAC）地址。 主 NIC 组成员是操作系统从一组初始团队成员中选择的网络适配器。
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 26d105e0-afc3-44b5-bb5e-0c884a4c5d62
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3ff03dae44600ff79ed22d298ee338c570e61e36
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405482"
---
# <a name="nic-teaming-mac-address-use-and-management"></a>NIC 组合 MAC 地址的使用和管理

>适用于：Windows Server 2016

当你使用 "交换机独立" 模式配置 NIC 组，并使用 "地址哈希" 或 "动态负载" 分发时，团队将在出站流量上使用主要 NIC 组成员的媒体访问控制（MAC）地址。 主 NIC 组成员是操作系统从一组初始团队成员中选择的网络适配器。  这是在创建团队之后或在重新启动主计算机后要绑定到团队的第一个团队成员。 由于主要团队成员可能在每次启动时以非确定性的方式更改、NIC 禁用/启用操作或其他重新配置活动，因此主要团队成员可能会发生更改，并且团队的 MAC 地址可能会有所不同。  
  
在大多数情况下，这不会造成问题，但在某些情况下可能会出现问题。  
  
如果从团队中删除主团队成员，然后将其置于操作中，则可能存在 MAC 地址冲突。 若要解决此冲突，请先禁用然后再启用团队界面。 禁用然后启用团队界面的过程会导致接口从其他团队成员那里选择新的 MAC 地址，从而消除了 MAC 地址冲突。  
  
可以通过在主组接口中设置 NIC 组的 mac 地址，将其设置为特定 MAC 地址，就像配置任何物理 NIC 的 MAC 地址时一样。  
  
## <a name="mac-address-use-on-transmitted-packets"></a>MAC 地址在传输的数据包上使用  
当你在 "交换机独立" 模式下配置 NIC 组，并使用 "地址哈希" 或 "动态负载" 分发时，来自单个源（如单个 VM）的数据包将同时分布在多个团队成员中。 若要防止交换机出现混淆，并阻止 MAC 稳定告警，则源 MAC 地址会替换为在主要团队成员以外的团队成员上传输的帧上的不同 MAC 地址。 因此，每个团队成员使用不同的 MAC 地址，并阻止 MAC 地址冲突，除非发生故障。  
  
当在主 NIC 上检测到故障时，NIC 组合软件会开始使用被选为临时主要团队成员的团队成员上的主要团队成员的 MAC 地址（也就是说，现在会将其作为主要团队提供给交换机m)）。  此更改仅适用于要在主要团队成员的 MAC 地址作为其源 MAC 地址的主要团队成员上发送的流量。 其他流量将继续与发生故障之前使用的任何源 MAC 地址一起发送。  
  
以下列表描述了基于团队配置方式的 NIC 组合 MAC 地址替换行为：  
  
1.  **采用地址哈希分布的交换机独立模式**  
  
    -   所有 ARP 和 NS 数据包都发送到主要团队成员  
  
    -   除主组成员之外的其他 Nic 上发送的所有流量都发送到已修改的源 MAC 地址，以匹配其发送到的 NIC  
  
    -   在主要团队成员上发送的所有流量都将与原始源 MAC 地址（可能是团队的源 MAC 地址）一起发送。  
  
2.  **在具有 Hyper-v 端口分发的交换机独立模式下**  
  
    -   每个 vmSwitch 端口都关联给团队成员  
  
    -   在关联端口的团队成员上发送每个数据包  
  
    -   未完成源 MAC 替换  
  
3.  **采用动态分布的交换机独立模式**  
  
    -   每个 vmSwitch 端口都关联给团队成员  
  
    -   所有 ARP/NS 数据包都发送到关联端口的团队成员。  
  
    -   作为关联团队成员的团队成员发送的数据包没有源 MAC 地址替换  
  
    -   在关联团队成员以外的团队成员上发送的数据包将进行源 MAC 地址替换  
  
4.  **处于交换机相关模式（所有分布区）**  
  
    -   不会执行源 MAC 地址替换  
  
## <a name="related-topics"></a>相关主题
- [NIC 组合](NIC-Teaming.md)：在本主题中，我们将概述 Windows Server 2016 中的网络接口卡（NIC）组合。 NIC 组合允许在一个或多个基于软件的虚拟网络适配器之间分组到32物理以太网网络适配器。 这些虚拟网络适配器可以提高性能，并在网络适配器发生故障时提供容错能力。  

- [NIC 组合设置](nic-teaming-settings.md)：在本主题中，我们将为你概述 NIC 组属性，例如组合和负载平衡模式。 此外，我们还会向你介绍备用适配器设置和主团队接口属性的详细信息。 如果 NIC 组中至少有两个网络适配器，则无需指定备用适配器来实现容错。
  
- [在主计算机或 VM 上创建新的 NIC 组](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md)：在本主题中，你将在主计算机或运行 Windows Server 2016 的 Hyper-v 虚拟机（VM）中创建新的 NIC 组。

- [NIC 组合故障排除](Troubleshooting-NIC-Teaming.md)：在本主题中，我们将讨论使用 Windows PowerShell 对 NIC 组合（如硬件、物理交换机证券和禁用或启用网络适配器）进行故障排除的方法。 
  


