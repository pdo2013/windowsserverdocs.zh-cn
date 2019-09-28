---
title: 规划 vRSS 的使用
description: 你可以使用本主题来准备虚拟机和 Hyper-v 主机，以便使用 Windows Server 2016 中的 vRSS。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 695e6192-5e84-4ab4-b33e-8ebf6b8f5cbb
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/04/2018
ms.openlocfilehash: 3addb500a654ff9f23c56388fec3ef2c3855a1da
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395819"
---
# <a name="plan-the-use-of-vrss"></a>规划 vRSS 的使用

>适用于：Windows Server（半年频道）、Windows Server 2016

在 Windows Server 2016 中，vRSS 在默认情况下处于启用状态，但是，你必须准备好环境，使 vRSS 在虚拟机中正常运行 \(VM @ no__t-1 或主机虚拟适配器上 \(vNIC @ no__t。 在 Windows Server 2012 R2 中，默认情况下已禁用 vRSS。

当你计划并准备好使用 vRSS 时，请确保：

- 物理网络适配器与虚拟机队列 \(VMQ @ no__t 兼容，且链接速度为 10 Gbps 或更高。
- 在物理 NIC 和超级 @ no__t-0V 虚拟交换机端口上启用 VMQ
- 没有为 VM 配置的单个 Root 输入 @ no__t-0Output Virtualization \(SR @ no__t-2IOV @ no__t。
- 已正确配置 NIC 组合。
- VM 具有多个逻辑处理器 \(LPs @ no__t-1。

>[!NOTE]
>默认情况下，已启用 RSS 的任何主机 Vnic 都启用了 vRSS。

下面是完成这些准备步骤所需的其他信息。
  
1. **网络适配器容量**。 验证网络适配器是否与虚拟机队列 \(VMQ @ no__t 兼容，且链接速度为 10 Gbps 或更高。 如果链接速度小于 10 Gbps，则默认情况下，超级 @ no__t-0V 虚拟交换机会禁用 VMQ，即使它仍在 Windows PowerShell 命令**get-netadaptervmq**的结果中显示 "已启用"。 可以用来验证是否已启用或禁用 VMQ 的一种方法是使用命令**get-netadaptervmqqueue**。  如果已禁用 VMQ，此命令的结果将显示没有 QueueID 分配给 VM 或主机虚拟网络适配器。 
  
2. **启用 VMQ**。 在主机上验证是否已启用 VMQ。 如果主机不支持 VMQ，则 vRSS 不起作用。 可以通过运行**获取-VMSwitch**并找到虚拟交换机正在使用的适配器来验证是否已启用 VMQ。 接下来，运行 **Get-NetAdapterVmq** 并确保该适配器显示在结果中并已启用 VMQ。
  
3. **缺少 SR @ no__t-1IOV**。 验证单个根输入 @ no__t-0Output Virtualization \(SR @ no__t-2IOV @ no__t \(VF @ no__t * 驱动程序未连接到 VM 网络接口。 可以使用**get-netadaptersriov**命令对此进行验证。 如果加载了某个 VF 驱动程序，RSS 将使用此驱动程序中的缩放设置，而不是由 vRSS 配置的设置。 如果 VF 驱动程序不支持 RSS，则会禁用 vRSS。
  
4. **NIC 组合配置**。 如果使用的是 NIC 组合，则必须正确配置 VMQ 才能使用 NIC 组合设置。 有关 NIC 组合部署和管理的详细信息，请参阅[Nic 组合](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming)。

5. **LPs 的数目**。 验证 VM 是否有多个逻辑处理器 \(LP @ no__t-1。 vRSS 依赖于 VM 或 Hyper-v 主机上的 RSS，对接收到多个 LPs 的流量进行负载均衡，以便进行并行处理。 可以通过在主机中运行 Windows PowerShell 命令**VMProcessor**来观察 VM 有多少 LPs。 运行该命令后，可以观察 LPs 数的计数列项。

主机 vNIC 始终有权访问所有物理处理器;若要将主机 vNIC 配置为使用特定数量的处理器，请在运行**Get-netadapterrss** Windows PowerShell 命令时使用 settings **-BaseProcessorNumber**和 **-MaxProcessors** 。

---