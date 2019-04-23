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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850438"
---
# <a name="plan-the-use-of-vrss"></a>规划 vRSS 使用

>适用于：Windows 服务器 （半年频道），Windows Server 2016

在 Windows Server 2016 中，启用 vRSS 默认情况下，但您必须准备好环境以允许才能正确运行的虚拟机中的 vRSS \(VM\)或在主机虚拟适配器\(vNIC\)。 在 Windows Server 2012 R2 中，默认情况下已禁用 vRSS。

在规划和准备 vRSS 使用，确保：

- 物理网络适配器，与虚拟机队列兼容\(VMQ\)和有链接速度为 10 Gbps 或更高。
- 和超物理 NIC 上启用 VMQ\-V 虚拟交换机端口
- 没有单个根输入\-输出虚拟化\(SR\-IOV\)为 VM 配置。
- 正确配置 NIC 组合。
- VM 具有多个逻辑处理器\(LPs\)。

>[!NOTE]
>默认情况下，为已启用 RSS 任何主机 Vnic 还启用 vRSS。

下面是完成这些准备步骤所需的其他信息。
  
1. **网络适配器容量**。 验证网络适配器是否与虚拟机队列兼容\(VMQ\)和有链接速度为 10 Gbps 或更高。 链接速度小于 10 Gbps，超\-虚拟机默认情况下，禁用 VMQ，即使它为已启用 Windows PowerShell 命令的结果仍显示 VMQ **Get-netadaptervmq**。 可用于验证启用或禁用 VMQ 的一种方法是使用命令**Get-netadaptervmqqueue**。  如果禁用 VMQ，则此命令的结果显示没有分配给 VM 或主机虚拟网络适配器没有 QueueID。 
  
2. **启用 VMQ**。 在主机上验证是否已启用 VMQ。 如果主机不支持 VMQ vRSS 无效。 你可以验证是否通过运行已启用 VMQ **Get-vmswitch**并查找使用虚拟交换机的适配器。 接下来，运行 **Get-NetAdapterVmq** 并确保该适配器显示在结果中并已启用 VMQ。
  
3. **缺少 SR\-IOV**。 验证单根输入\-输出虚拟化\(SR\-IOV\)虚函数\(VF\)驱动程序未附加到 VM 网络接口。 您可以通过使用对此进行验证**Get-netadaptersriov**命令。 如果加载某个 VF 驱动程序，则 RSS 而不是配置的 vRSS 使用此驱动程序中的缩放设置。 如果 VF 驱动程序不支持 RSS，则会禁用 vRSS。
  
4. **NIC 组合配置**。 如果使用 NIC 组合，务必正确配置 VMQ，从而与 NIC 组合设置一起使用。 有关 NIC 组合部署和管理的详细信息，请参阅[NIC 组合](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming)。

5. **数 LPs**。 验证 VM 有多个逻辑处理器\(LP\)。 vRSS 依赖于在 VM 中的 RSS 或流量进行负载平衡接收到的并行处理多个 LPs 的 HYPER-V 主机上。 您可以观察 VM 已通过运行 Windows PowerShell 命令的多少 LPs **Get-vmprocessor**主机中。 运行该命令后，可以为 LPs 数观察计数列条目。

主机 vNIC 始终有权访问的所有物理处理器;若要配置主机 vNIC 以使用特定数目的处理器，使用的设置 **-BaseProcessorNumber**并 **-MaxProcessors**在运行时**Set-netadapterrss**Windows PowerShell 命令。

---