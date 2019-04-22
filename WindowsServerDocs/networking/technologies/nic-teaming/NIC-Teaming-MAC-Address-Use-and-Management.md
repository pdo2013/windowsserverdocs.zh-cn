---
title: NIC 组合 MAC 地址的使用和管理
description: 在 NIC 组配置为切换独立模式和地址哈希或动态负载分发时，该团队使用的媒体访问控制 (MAC) 地址的出站流量上的主 NIC 组成员。 主 NIC 团队成员是由操作系统来自团队成员的初始集选择的网络适配器。
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 26d105e0-afc3-44b5-bb5e-0c884a4c5d62
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 074a55d2f0a5423af5892ed73dc8a2b8dbda9d5b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824248"
---
# <a name="nic-teaming-mac-address-use-and-management"></a>NIC 组合 MAC 地址的使用和管理

>适用于：Windows Server 2016

在 NIC 组配置为切换独立模式和地址哈希或动态负载分发时，该团队使用的媒体访问控制 (MAC) 地址的出站流量上的主 NIC 组成员。 主 NIC 团队成员是由操作系统来自团队成员的初始集选择的网络适配器。  它是在创建后或在主计算机重新启动后将绑定到团队的第一个团队成员。 因为主要团队成员可能会更改以确定性方式在每个启动，NIC 禁用/启用操作或其他的重新配置活动，主要的团队成员可能会更改和团队的 MAC 地址可能有所不同。  
  
在大多数情况下，这不会导致问题，但有少数情况下可能会出现问题。  
  
如果从团队中删除，然后放入操作的主要团队成员可能有 MAC 地址冲突。 若要解决此冲突，禁用，然后启用组接口。 禁用并启用组接口的过程会导致要从剩余的团队成员，从而消除 MAC 地址冲突选择新的 MAC 地址的接口。  
  
您可以设置 MAC 地址的 NIC 组的到特定的 MAC 地址设置中的主要组接口，就像可以在配置的 MAC 地址的任何物理 nic。  
  
## <a name="mac-address-use-on-transmitted-packets"></a>在传输数据包上使用 MAC 地址  
在 NIC 组配置中切换独立模式和地址哈希或动态负载分发时，来自单个源 （如单个 VM) 的数据包同时会分布在多个团队成员。 以防止交换机感到迷惑并防止 MAC 不稳定警报，源 MAC 地址替换为在非主要组成员的团队成员上传输的帧上不同的 MAC 地址。 因此，每个团队成员使用不同的 MAC 地址，而 MAC 地址冲突会阻止，直到发生故障。  
  
主 NIC 上检测到故障时，NIC 组合软件开始使用上选择作为临时的主要组成员 （即，一个将立即显示到交换机为主要组我的团队成员主要团队成员的 MAC 地址mber)。  此更改仅适用于已将要发送与主要团队成员的 MAC 地址的主要团队成员作为其源 MAC 地址的流量。 其他流量可继续与任何源故障发生之前就会使用 MAC 地址一起发送。  
  
以下是描述基于团队的配置方式的 NIC 组合 MAC 地址替换行为的列表：  
  
1.  **在独立于交换机模式下使用地址哈希分布**  
  
    -   所有 ARP 和 NS 数据包都发送上主要组成员  
  
    -   Nic 上发送的主要团队成员以外的所有流量都发送与修改以匹配在其都发送的 NIC 的源 MAC 地址  
  
    -   主要组成员上发送的所有流量都发送与原始的源 MAC 地址 （这可能是团队的源 MAC 地址）  
  
2.  **在独立于交换机模式下的 HYPER-V 端口分发**  
  
    -   每个 vmSwitch 端口关联到团队成员  
  
    -   每个数据包发送端口关联到的团队成员对  
  
    -   不完成任何源 MAC 替换  
  
3.  **在独立于交换机模式下使用动态分配**  
  
    -   每个 vmSwitch 端口关联到团队成员  
  
    -   发送端口关联到的团队成员对所有 ARP/NS 数据包  
  
    -   是关联的团队成员的团队成员对发送的数据包将没有源 MAC 地址替换完成  
  
    -   上的团队成员不关联的团队成员发送的数据包将已完成的源 MAC 地址替换  
  
4.  **在依赖于交换机模式下 （所有分布区）**  
  
    -   执行没有源 MAC 地址替换  
  
## <a name="related-topics"></a>相关主题
- [NIC 组合](NIC-Teaming.md):在本主题中，我们提供您的网络接口卡 (NIC) 合作概述 Windows Server 2016 中。 NIC 组合允许你将介于 1 和 32 之间分组到一个或多个基于软件的虚拟网络适配器的物理以太网网络适配器。 这些虚拟网络适配器可以提高性能，并在网络适配器发生故障时提供容错能力。  

- [NIC 组合设置](nic-teaming-settings.md):在本主题中，我们为您提供的 NIC 组属性，如组合概述和负载平衡模式。 我们还会为您提供的备用适配器设置和主要组接口属性的详细信息。 如果 NIC 组中具有至少两个网络适配器，您不需要指定的容错能力的备用适配器。
  
- [在主计算机或 VM 上创建新的 NIC 组](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md):本主题中的主计算机上或在运行 Windows Server 2016 的 HYPER-V 虚拟机 (VM) 中创建新的 NIC 组。

- [NIC 组合疑难解答](Troubleshooting-NIC-Teaming.md):在本主题中，我们将讨论如何解决 NIC 组合，如硬件、 物理交换机证券和使用 Windows PowerShell 的禁用或启用网络适配器。 
  


