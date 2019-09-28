---
title: 软件定义的网络中的 HNV 网关性能优化
description: 有关软件定义的网络的 HNV 网关性能优化指南
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: grcusanz; AnPaul
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 907b160b143af18a8ede3a9a7975fa8b22753118
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383503"
---
# <a name="hnv-gateway-performance-tuning-in-software-defined-networks"></a>软件定义的网络中的 HNV 网关性能优化

除了 Windows Server 网关虚拟机（Vm）的配置参数外，本主题还为运行 Hyper-v 和托管 Windows Server 网关虚拟机的服务器提供硬件规范和配置建议. 若要从 Windows Server 网关 Vm 提取最佳性能，应遵循这些指导原则。
下列部分包含部署 Windows Server 网关时需满足的硬件和配置要求。
1. 有关 Hyper-V 硬件的建议
2. Hyper-V 主机配置
3. Windows Server 网关 VM 配置

## <a name="hyper-v-hardware-recommendations"></a>有关 Hyper-V 硬件的建议

下面是运行 Windows Server 2016 和 Hyper-v 的每台服务器的最低建议硬件配置。

| 服务器组件               | 规格                                                                                                                                                                                                                                                                   |
|--------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 中心处理单元 (CPU)  | 非统一内存体系结构（NUMA）节点：2 <br> 如果主机上有多个 Windows Server 网关 Vm，为了获得最佳性能，每个网关 VM 应具有对一个 NUMA 节点的完全访问权限。 它应该与主机物理适配器使用的 NUMA 节点不同。 |
| 每个 NUMA 节点的核心数            | 2                                                                                                                                                                                                                                                                               |
| 超线程                | 已禁用。 超线程不能提高 Windows Server 网关的性能。                                                                                                                                                                                           |
| 随机存取内存 (RAM)     | 48 GB。                                                                                                                                                                                                                                                                           |
| 网络接口卡 (NIC) | 2 10 GB Nic，则网关性能将取决于线路速率。 如果线路速率小于10Gbps，则网关隧道吞吐量数值也会按同样的因素下降。                                                                                          |

确保分配给 Windows Server 网关 VM 的虚拟处理器数不超过 NUMA 节点上的处理器数。 例如，如果 NUMA 节点有 8 个核心，则虚拟处理器数应小于或等于 8。 为了获得最佳性能，应为8。 若要找出 NUMA 节点数以及每个 NUMA 节点的核心数，请在每台 Hyper-V 主机上运行以下 Windows PowerShell 脚本：

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

如果每个 NUMA 节点有8个内核，则建议在每个 Hyper-v 主机上选择要在每个 Hyper-v 主机上安装的网关 vm 数量时，一个具有八个虚拟处理器和至少 8GB RAM 的网关 VM。 在这种情况下，一个 NUMA 节点专用于主计算机。

## <a name="hyper-v-host-configuration"></a>Hyper-v 主机配置

下面是运行 Windows Server 2016 和 Hyper-v 的每台服务器的建议配置，其工作负荷是运行 Windows Server 网关 Vm。 这些配置说明包括 Windows PowerShell 命令的用法示例。 这些示例包含了占位符，用于表示你在环境中运行这些命令时需要提供的实际值。 例如，网络适配器名称占位符为 "NIC1" 和 "NIC2"。 在运行使用这些占位符的命令时，请使用服务器上的网络适配器的实际名称，而不要使用占位符，否则命令将会失败。

>[!Note]
> 若要运行以下 Windows PowerShell 命令，你必须是管理员组的成员。

| 配置项目                          | Windows Powershell 配置                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 交换机嵌入式组合                     | 创建具有多个网络适配器的 vswitch 时，它会自动为这些适配器启用交换机嵌入组合。 <br> ```New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2"``` <br> Windows Server 2016 中的 SDN 不支持通过 LBFO 的传统组合。 交换机嵌入组合允许你将同一组 Nic 用于虚拟流量和 RDMA 流量。 基于 LBFO 的 NIC 组合不支持此操作。                                                        |
| 物理 NIC 上的中断裁决       | 使用默认设置。 若要检查配置，可以使用以下 Windows PowerShell 命令： ```Get-NetAdapterAdvancedProperty```                                                                                                                                                                                                                                                                                                                                                                    |
| 物理 NIC 上的接收缓冲区大小       | 可以通过运行命令 ```Get-NetAdapterAdvancedProperty``` 来验证物理 Nic 是否支持此参数的配置。 如果它们不支持此参数，则该命令的输出将不包含属性 "接收缓冲区"。 如果 NIC 确实支持此参数，你可以使用以下 Windows PowerShell 命令设置接收缓冲区大小： <br>```Set-NetAdapterAdvancedProperty "NIC1" –DisplayName "Receive Buffers" –DisplayValue 3000``` <br>                          |
| 物理 NIC 上的发送缓冲区大小          | 可以通过运行命令 ```Get-NetAdapterAdvancedProperty``` 来验证物理 Nic 是否支持此参数的配置。 如果 Nic 不支持此参数，该命令的输出将不包含属性 "发送缓冲区"。 如果 NIC 确实支持此参数，你可以使用以下 Windows PowerShell 命令设置发送缓冲区大小： <br> ```Set-NetAdapterAdvancedProperty "NIC1" –DisplayName "Transmit Buffers" –DisplayValue 3000``` <br>                           |
| 物理 NIC 上的接收方缩放 (RSS) | 可以通过运行 Windows PowerShell 命令 Get-netadapterrss 来验证物理 Nic 是否已启用 RSS。 你可以使用以下 Windows PowerShell 命令来启用和配置网络适配器上的 RSS： <br> ```Enable-NetAdapterRss "NIC1","NIC2"```<br> ```Set-NetAdapterRss "NIC1","NIC2" –NumberOfReceiveQueues 16 -MaxProcessors``` <br> 注意：如果启用 VMMQ 或 VMQ，则不需要在物理网络适配器上启用 RSS。 你可以在主机虚拟网络适配器上启用它。 |
| VMMQ                                        | 若要为 VM 启用 VMMQ，请运行以下命令： <br> ```Set-VmNetworkAdapter -VMName <gateway vm name>,-VrssEnabled $true -VmmqEnabled $true``` <br> 注意：并非所有网络适配器都支持 VMMQ。 目前，它在 Chelsio T5 和 T6、Mellanox CX-3 和 CX-4 以及 QLogic 45xxx 系列上受支持                                                                                                                                                                                                                                      |
| NIC 组上的虚拟机队列 (VMQ) | 你可以使用以下 Windows PowerShell 命令在集团队上启用 VMQ： <br>```Enable-NetAdapterVmq``` <br> 注意：仅当 HW 不支持 VMMQ 时，才应启用此功能。 如果支持，则应启用 VMMQ 以提高性能。                                                                                                                                                                                                                                                               |
>[!Note]
> 仅当 VM 上的负载较高且 CPU 使用率达到最大值时，VMQ 和 vRSS 才会进入图片。 只有一个处理器核心数至少为1。对于跨多个内核分散处理负载，VMQ 和 vRSS 将非常有用。 这不适用于 IPsec 流量，因为 IPsec 流量限制为单个核心。

## <a name="windows-server-gateway-vm-configuration"></a>Windows Server 网关 VM 配置

在这两个 Hyper-v 主机上，你可以配置多个配置为带有 Windows Server 网关的网关的 Vm。 在 Hyper-V 主机上，可以使用虚拟交换机管理器创建一个与 NIC 组绑定的 Hyper-V 虚拟交换机。 请注意，为了获得最佳性能，应在 Hyper-v 主机上部署单个网关 VM。
以下是每个 Windows Server 网关 VM 的建议配置。

| 配置项目                 | Windows Powershell 配置                                                                                                                                                                                                                                                                                                                                                               |
|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 内存                             | 8 GB                                                                                                                                                                                                                                                                                                                                                                                           |
| 虚拟网络适配器数 | 3个 Nic，具体用途如下：1表示管理操作系统使用的管理，1个外部网络提供对外部网络的访问权限，1表示仅提供对内部网络的访问。                                                                                                                                                            |
| Receive Side Scaling (RSS)         | 你可以保留管理 NIC 的默认 RSS 设置。 以下示例配置适用于具有 8 个虚拟处理器的 VM。 对于外部和内部 Nic，可以使用以下 Windows PowerShell 命令，通过将 BaseProcNumber 设置为0，并将 MaxRssProcessors 设置为8来启用 RSS： <br> ```Set-NetAdapterRss "Internal","External" –BaseProcNumber 0 –MaxProcessorNumber 8``` <br> |
| 发送端缓冲区                   | 你可以保留管理 NIC 的默认发送端缓冲区设置。 对于内部和外部 Nic，可以使用以下 Windows PowerShell 命令来配置包含 32 MB RAM 的发送端缓冲区： <br> ```Set-NetAdapterAdvancedProperty "Internal","External" –DisplayName "Send Buffer Size" –DisplayValue "32MB"``` <br>                                                       |
| 接收端缓冲区                | 你可以保留管理 NIC 的默认接收端缓冲区设置。 对于内部和外部 Nic，可以使用以下 Windows PowerShell 命令，通过 16 MB 的 RAM 配置接收端缓冲区： <br> ```Set-NetAdapterAdvancedProperty "Internal","External" –DisplayName "Receive Buffer Size" –DisplayValue "16MB"``` <br>                                            |
| 转发优化               | 你可以保留管理 NIC 的默认前向优化设置。 对于内部和外部 Nic，可以使用以下 Windows PowerShell 命令启用前向优化： <br> ```Set-NetAdapterAdvancedProperty "Internal","External" –DisplayName "Forward Optimization" –DisplayValue "1"``` <br>                                                                      |
