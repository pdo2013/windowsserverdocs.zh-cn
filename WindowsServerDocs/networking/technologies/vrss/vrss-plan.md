---
title: 规划 vRSS 使用
description: 本主题可用于在 Windows Server 2016 中使用 vRSS 准备你的虚拟机和 HYPER-V 主机。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 695e6192-5e84-4ab4-b33e-8ebf6b8f5cbb
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/04/2018
ms.openlocfilehash: e6558b00e87721d8ab81c84946a14745c4faa812
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133383"
---
# 规划 vRSS 使用

>适用于：Windows Server（半年频道）、Windows Server 2016

在 Windows Server 2016 中，vRSS 为启用默认情况下，但你必须准备你的环境，以允许 vRSS 正常运行的虚拟机中 \(VM\) 或在主机虚拟适配器 \(vNIC\) 上。 在 Windows Server 2012 R2，默认情况下已禁用 vRSS。

当你规划和准备 vRSS 使用时，请确保：

- 物理网络适配器与虚拟机队列 \(VMQ\) 兼容，并且具有 10 Gbps 的链接速度或的详细信息。
- 和 HYPER-V 虚拟交换机端口上的物理 NIC VMQ 已启用
- 配置的虚拟机没有单个根 Input\ 输出虚拟化 \(SR\-IOV\)。
- NIC 组合已正确配置。
- 虚拟机具有多个逻辑处理器 \(LPs\)。

>[!NOTE]
>默认情况下，对于已启用的 RSS 任何主机 vNICs 还启用 vRSS。

以下是你需要完成这些准备步骤的其他信息。
  
1. **网络适配器容量**。 验证网络适配器与虚拟机队列 \(VMQ\) 兼容，并且具有 10 Gbps 或多个链接速度。 如果链接速度小于 10 Gbps，HYPER-V 虚拟交换机禁用默认情况下，VMQ，即使它仍显示 VMQ 为在 Windows PowerShell 命令**获取 NetAdapterVmq**的结果中启用。 你可以使用验证 VMQ 是启用还是禁用的一种方法是使用命令**获取 NetAdapterVmqQueue**。  如果 VMQ 处于禁用状态，此命令的结果将显示不是否存在任何 QueueID 分配给 VM 或主机虚拟网络适配器。 
  
2. **启用 VMQ**。 验证主机上启用了 VMQ。 如果主机不支持 VMQ vRSS 无效。 你可以验证 VMQ 的启用方式运行**获取 VMSwitch**和查找使用的虚拟交换机的适配器。 接下来，运行**获取 NetAdapterVmq** ，并确保在结果中显示适配器，并启用了 VMQ。
  
3. **缺少 SR\ IOV**。 验证单个根 Input\ 输出虚拟化 \(SR\-IOV\) 虚拟函数 \(VF\) 驱动程序无法连接到虚拟机网络接口。 你可以通过**获取 NetAdapterSriov**命令对此进行验证。 如果加载 VF 驱动程序时，RSS 而不是通过 vRSS 配置使用此驱动程序的缩放设置。 如果 VF 驱动程序不支持 RSS，则会禁用 vRSS。
  
4. **NIC 组合配置**。 如果你使用 NIC 组合，务必正确配置 VMQ 要使用的 NIC 组合的设置。 有关 NIC 组合部署和管理的详细信息，请参阅[NIC 组合](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming)。

5. **Lp 的数量**。 验证 VM 是否有多个逻辑处理器 \(LP\)。 vRSS 依赖于在 VM 中的 RSS 或在 HYPER-V 主机上用于加载到并行处理多个 Lp 平衡收到流量。 你可以观察多少 Lp 你的虚拟机已通过在主机中运行的 Windows PowerShell 命令**获取 VMProcessor** 。 运行此命令后，可以用于数量的 Lp 观察计数列条目。

主机 vNIC 始终有权访问的所有物理处理器;若要配置主机 vNIC 以使用特定的处理器数，请使用设置 **-BaseProcessorNumber**和 **-MaxProcessors**时运行的**一组 NetAdapterRss** Windows PowerShell 命令。

---