---
title: NIC 分组 MAC 地址使用和管理
description: 本主题提供有关如何 NIC 组合使用媒体访问控制 (MAC) 地址在 Windows Server 2016 的信息。
manager: brianlic
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
ms.openlocfilehash: 6ab624665c964b100e6b1ed633120299b55e37df
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="nic-teaming-mac-address-use-and-management"></a>NIC 分组 MAC 地址使用和管理

>适用于：Windows Server 2016

配置时 NIC 团队切换独立模式和地址哈希或负载动态分配，团队使用媒体访问控制 (MAC) 地址主要 NIC 团队成员站的交通。 主要 NIC 团队成员是由操作系统团队成员初始设置从所选的网络适配器。  
  
主要组成员是第一个团队成员创建它或主计算机重启后，将向团队。 主要组成员可能会在每次启动不确定方式更改，因为 NIC 禁用/启用操作或其他重新配置活动，主要组成员可能会更改，并团队的 MAC 地址可能会有所不同。  
  
在大多数情况下，这不会导致问题，但少数情况下可能会出现问题。  
  
如果主要组成员是从应用团队中删除，然后置于操作可能有 MAC 地址冲突。 若要解决该冲突，禁用，然后启用团队界面。 禁用，然后启用团队接口的过程将导致界面，以从其余团队的成员，从而消除 MAC 地址冲突选择新的 MAC 地址。  
  
你可以设置 NIC 团队的 MAC 地址到特定的 MAC 地址将其设置中的主团队界面，就像配置的任何物理 nic。的 MAC 地址时，你可以执行  
  
## <a name="mac-address-use-on-transmitted-packets"></a>在传输数据包上使用的 MAC 地址  
当你切换独立模式和地址哈希或负载动态分配配置 NIC 团队时，从单个源（例如一个单独的 VM) 数据包同时分布多个团队的成员。 若要防止切换的混淆，以防止 MAC 翅膀闹钟源的 MAC 地址将替换上之外的主要团队成员团队成员传输帧的不同的 MAC 地址。 出于此原因，每个团队成员使用不同的 MAC 地址，并，直到出现故障，将阻止 MAC 地址冲突。  
  
主要 NIC 上检测到时出现故障，NIC 组合软件启动上选择要作为主要的临时组成员（即，现在将显示为主要组成员切换到的一个）的团队成员使用主要组成员的 MAC 地址。  此更改仅适用于将发送主要组成员的 MAC 地址与主要组成员作为 MAC 地址其源的通信。 继续发送任何源会失败之前已使用的 MAC 地址与其他交通。  
  
以下是描述 NIC 组合 MAC 地址更换行为基于配置团队的方式的列表：  
  
1.  **在地址哈希 distribution 独立切换模式**  
  
    -   所有 ARP 和 NS 数据包都发送上的主要团队成员  
  
    -   之外的主要团队成员发送 Nic 上的所有通信是修改匹配它们都发送 NIC 来源 MAC 地址与已都发送  
  
    -   与原始源（这可能团队的源的 MAC 地址）的 MAC 地址发送上的主要团队成员发送的所有通信  
  
2.  **在 Hyper-V 端口 distribution 带的独立切换模式**  
  
    -   每个 vmSwitch 端口依附给团队成员  
  
    -   团队成员依附端口是指每个数据包  
  
    -   完成 MAC 更换没有源  
  
3.  **在切换独立模式动态分配**  
  
    -   每个 vmSwitch 端口依附给团队成员  
  
    -   打开端口依附向其团队成员发送所有 ARP/NS 数据包  
  
    -   上是关联的团队成员团队成员发送的数据包已完成的 MAC 地址更换没有源  
  
    -   上以外的关联的团队成员团队成员发送的数据包将有源 MAC 地址更换完成  
  
4.  **在任何依赖切换模式下（所有分配）**  
  
    -   执行 MAC 地址更换没有源  
  
## <a name="see-also"></a>请参阅  
[在主计算机或 VM 中创建新的 NIC 团队](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md)  
[NIC 组合](NIC-Teaming.md)  
  


