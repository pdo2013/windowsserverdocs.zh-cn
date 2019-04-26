---
title: HNV 网关性能优化中软件定义网络
description: HNV 网关性能优化指南软件定义网络
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: grcusanz; AnPaul
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 217428b84a00b2e2231a15cb3878d0abcec1d9ad
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827318"
---
# <a name="hnv-gateway-performance-tuning-in-software-defined-networks"></a>HNV 网关性能优化中软件定义网络

本主题提供了硬件规格和服务器正在运行 HYPER-V 且托管 Windows Server 网关虚拟机，除了 Windows Server 网关虚拟机 (Vm) 的配置参数的配置建议. 若要从 Windows Server 网关 Vm 提取获得最佳性能，应将遵循这些准则。
下列部分包含部署 Windows Server 网关时需满足的硬件和配置要求。
1. 有关 Hyper-V 硬件的建议
2. Hyper-V 主机配置
3. Windows Server 网关 VM 配置

## <a name="hyper-v-hardware-recommendations"></a>有关 Hyper-V 硬件的建议

下面是每个正在运行 Windows Server 2016 和 HYPER-V 的服务器的建议最低硬件配置。

| 服务器组件               | 规格                                                                                                                                                                                                                                                                   |
|--------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 中心处理单元 (CPU)  | 非统一内存体系结构 (NUMA) 节点：2 <br> 如果有多个主机，为了获得最佳性能上的 Windows Server 网关 Vm 每个网关 VM 应具有一个 NUMA 节点的完全访问权限。 它应不同于使用的主机物理适配器的 NUMA 节点。 |
| 每个 NUMA 节点的核心数            | 2                                                                                                                                                                                                                                                                               |
| Hyper-Threading                | 已禁用。 超线程不能提高 Windows Server 网关的性能。                                                                                                                                                                                           |
| 随机存取内存 (RAM)     | 48 GB。                                                                                                                                                                                                                                                                           |
| 网络接口卡 (NIC) | 两个 10 GB Nic，网关性能将依赖于行速率。 如果行速率小于 10Gbps，网关隧道吞吐量数字也会关闭由相同的系数。                                                                                          |

确保分配给 Windows Server 网关 VM 的虚拟处理器数不超过 NUMA 节点上的处理器数。 例如，如果 NUMA 节点有 8 个核心，则虚拟处理器数应小于或等于 8。 为了获得最佳性能，它应为 8。 若要找出 NUMA 节点数以及每个 NUMA 节点的核心数，请在每台 Hyper-V 主机上运行以下 Windows PowerShell 脚本：

```PowerShell
$nodes = [object[]] $(gwmi –Namespace root\virtualization\v2 -Class MSVM_NumaNode)
$cores = ($nodes | Measure-Object NumberOfProcessorCores -sum).Sum
$lps = ($nodes | Measure-Object NumberOfLogicalProcessors -sum).Sum


Write-Host "Number of NUMA Nodes: ", $nodes.count
Write-Host ("Total Number of Cores: ", $cores)
Write-Host ("Total Number of Logical Processors: ", $lps)
```

>[!Important]
> 跨 NUMA 节点分配虚拟处理器可能会给 Windows Server 网关的性能造成负面影响。 运行多个 VM 并让每个 VM 使用一个 NUMA 节点的虚拟处理器的做法所产生的聚合性能，很有可能优于运行一个 VM 并向其分配所有虚拟处理器的做法。

一个网关 VM 有 8 个虚拟处理器和至少 8 GB RAM 时，建议选择网关 Vm 的数量时每个 NUMA 节点有八个核心，每个 HYPER-V 主机上安装。 在这种情况下，一个 NUMA 节点专用于主机计算机。

## <a name="hyper-v-host-configuration"></a>HYPER-V 主机配置

下面是运行 Windows Server 2016 和 HYPER-V 和它的每个服务器的建议的配置工作负荷是运行 Windows Server 网关 Vm。 这些配置说明包括 Windows PowerShell 命令的用法示例。 这些示例包含了占位符，用于表示你在环境中运行这些命令时需要提供的实际值。 例如，网络适配器名称占位符为"NIC1？ 和"NIC2。？ 在运行使用这些占位符的命令时，请使用服务器上的网络适配器的实际名称，而不要使用占位符，否则命令将会失败。

>[!Note]
> 若要运行以下 Windows PowerShell 命令，你必须是管理员组的成员。

| 配置项目                          | Windows Powershell 配置                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 交换机嵌入式组合                     | 当使用多个网络适配器创建 vswitch 时，它会自动启用交换机嵌入式组合这些适配器。 <br> ```New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2"``` <br> 不支持与 Windows Server 2016 中的 SDN 传统通过 LBFO 组合。 交换机嵌入式组合，可使用你的虚拟通信和 RDMA 通信的一组相同的 Nic。 与 NIC 组合基于 LBFO 不支持这样做。                                                        |
| 物理 NIC 上的中断裁决       | 使用默认设置。 若要检查的配置，可以使用以下 Windows PowerShell 命令： ```Get-NetAdapterAdvancedProperty```                                                                                                                                                                                                                                                                                                                                                                    |
| 物理 NIC 上的接收缓冲区大小       | 你可以验证物理 Nic 是否通过运行命令支持此参数的配置```Get-NetAdapterAdvancedProperty```。 如果它们不支持此参数，该命令的输出不包含属性"接收缓冲区。？ 如果 NIC 确实支持此参数，你可以使用以下 Windows PowerShell 命令设置接收缓冲区大小： <br>```Set-NetAdapterAdvancedProperty “NIC1�? –DisplayName “Receive Buffers�? –DisplayValue 3000``` <br>                          |
| 物理 NIC 上的发送缓冲区大小          | 你可以验证物理 Nic 是否通过运行命令支持此参数的配置```Get-NetAdapterAdvancedProperty```。 如果 Nic 确实支持此参数，该命令的输出不包含属性"发送缓冲区。？ 如果 NIC 确实支持此参数，你可以使用以下 Windows PowerShell 命令设置发送缓冲区大小： <br> ```Set-NetAdapterAdvancedProperty “NIC1�? –DisplayName “Transmit Buffers�? –DisplayValue 3000``` <br>                           |
| 物理 NIC 上的接收方缩放 (RSS) | 您可以验证物理 Nic 是否已通过运行 Windows PowerShell 命令 Get-netadapterrss 启用 RSS。 可以使用以下 Windows PowerShell 命令来启用和配置网络适配器上的 RSS: <br> ```Enable-NetAdapterRss “NIC1�?,�?NIC2�?```<br> ```Set-NetAdapterRss “NIC1�?,�?NIC2�? –NumberOfReceiveQueues 16 -MaxProcessors``` <br> 注意：如果启用 VMMQ 或 VMQ，则不必在物理网络适配器上启用 RSS。 您可以在主机虚拟网络适配器上启用它 |
| VMMQ                                        | 若要为 vm 启用 VMMQ，请运行以下命令： <br> ```Set-VmNetworkAdapter -VMName <gateway vm name>,-VrssEnabled $true -VmmqEnabled $true``` <br> 注意：并非所有网络适配器都支持 VMMQ。 目前，支持 Chelsio T5，T6，Mellanox CX 3 和 CX-4 和 QLogic 45xxx 系列上                                                                                                                                                                                                                                      |
| NIC 组上的虚拟机队列 (VMQ) | 可以使用以下 Windows PowerShell 命令集团队启用 VMQ: <br>```Enable-NetAdapterVmq``` <br> 注意：硬件不支持 VMMQ，才应启用此项。 如果支持，应启用更好的性能的 VMMQ。                                                                                                                                                                                                                                                               |
>[!Note]
> VMQ 和 vRSS 进入图片仅当在 VM 上的负载较高和最大程度利用 CPU。 只有这样将至少一个处理器核心 max 出。VMQ 和 vRSS 然后将有利，可帮助处理负载分布到多个内核。 这不适用于 IPsec 流量，因为 IPsec 流量仅限于单核。

## <a name="windows-server-gateway-vm-configuration"></a>Windows Server 网关 VM 配置

在这两个 HYPER-V 主机，可以配置多个配置为使用 Windows Server 网关的网关的 Vm。 在 Hyper-V 主机上，可以使用虚拟交换机管理器创建一个与 NIC 组绑定的 Hyper-V 虚拟交换机。 请注意，为了获得最佳性能，你应该部署单个网关 VM 的 HYPER-V 主机上。
以下是每个 Windows Server 网关 VM 的建议配置。

| 配置项目                 | Windows Powershell 配置                                                                                                                                                                                                                                                                                                                                                               |
|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 内存                             | 8 GB                                                                                                                                                                                                                                                                                                                                                                                           |
| 虚拟网络适配器数 | 使用具有以下特定的 3 个 Nic:1 个用于管理由管理操作系统的 1 个对外，提供对外部网络，1 个内部提供访问权限仅内部网络的访问。                                                                                                                                                            |
| Receive Side Scaling (RSS)         | 可以保留的默认 RSS 设置为管理 nic。 以下示例配置适用于具有 8 个虚拟处理器的 VM。 对于外部和内部 Nic，可以启用 RSS baseprocnumber 设置为 0、maxrssprocessors 设置为 8 使用以下 Windows PowerShell 命令： <br> ```Set-NetAdapterRss “Internal�?,�?External�? –BaseProcNumber 0 –MaxProcessorNumber 8``` <br> |
| 发送方缓冲区                   | 可以保留默认值为管理 NIC 的发送方缓冲区设置 对于内部和外部 Nic 可以发送方缓冲区配置使用 32 MB 的 RAM 使用以下 Windows PowerShell 命令： <br> ```Set-NetAdapterAdvancedProperty “Internal�?,�?External�? –DisplayName “Send Buffer Size�? –DisplayValue “32MB�?``` <br>                                                       |
| 接收方缓冲区                | 可以保留默认值为管理 NIC 的接收方缓冲区设置 对于内部和外部 Nic 上，可以使用 16 MB 的 RAM 配置接收方缓冲区，使用以下 Windows PowerShell 命令： <br> ```Set-NetAdapterAdvancedProperty “Internal�?,�?External�? –DisplayName “Receive Buffer Size�? –DisplayValue “16MB�?``` <br>                                            |
| 转发优化               | 可以保留的默认转发优化设置为管理 nic。 对于内部和外部 Nic，可以使用以下 Windows PowerShell 命令来启用转发优化： <br> ```Set-NetAdapterAdvancedProperty “Internal�?,�?External�? –DisplayName “Forward Optimization�? –DisplayValue “1�?``` <br>                                                                      |
