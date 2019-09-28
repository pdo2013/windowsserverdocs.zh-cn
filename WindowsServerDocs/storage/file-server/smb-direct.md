---
title: 使用 SMB 直通提高文件服务器的性能
description: 描述 Windows Server 2012 R2、Windows Server 2012 和 Windows Server 2016 中的 SMB 直接功能。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 41126aa0d054607449d57928c1777679e5087e73
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394457"
---
# <a name="smb-direct"></a>SMB 直通

>适用于：Windows Server 2012 R2、Windows Server 2012、Windows Server 2016

Windows Server 2012 R2、Windows Server 2012 和 Windows Server 2016 包含一项名为 SMB Direct 的功能，该功能支持使用具有远程直接内存访问（RDMA）功能的网络适配器。 使用 RDMA 的网络适配器能够全速运行， 延迟时间非常低，CPU 使用量非常少。 对于 Hyper-V 或 Microsoft SQL Server 等工作负载，这让远程文件服务器如同本地存储一样。 SMB 直通包括：

- 提高了吞吐量：利用高速网络的完整吞吐量，其中网络适配器以线速度协调大量数据的传输。
- 低延迟时间：提供极其快速的网络请求响应功能，因此使远程文件存储如同直接连接的模块存储功能一样易于操作。
- 低 CPU 使用率：在网络上传输数据时，占用较少 CPU 周期，从而为服务器应用程序保留更多空闲能量。

SMB 直通是由 Windows Server 2012 R2 和 Windows Server 2012 自动配置的。

## <a name="smb-multichannel-and-smb-direct"></a>SMB 多通道和 SMB 直通

SMB 多通道的功能是负责检测网络适配器 RDMA 功能以启用 SMB 直通。 如果未配置 SMB 多通道，则 SMB 使用常规 TCP/IP 与支持 RDMA 功能的网络适配器（所有网络适配器均提供 TCP/IP 堆栈和新的 RDMA 堆栈）。

SMB 使用 SMB 多通道检测网络适配器是否具有 RDMA 功能，然后为该单一会话创建多重 RDMA 连接（每个接口有两个）。 这允许 SMB 使用支持 RDMA-功能的网络适配器，从而提供高吞吐量、低延迟时间和较少 CPU 使用率这些功能。 此外，在使用多重 RDMA 接口时，它还具有容错功能。

>[!NOTE]
>如果您打算使用网络适配器 RDMA 功能，则不应组合支持 RDMA 功能的网络适配器。 组合时，网络适配器将不再支持 RDMA 功能。
>至少创建一个 RDMA 网络连接，不再使用 TCP/IP 连接（用于原始协议协商）。 然而，在 RDMA 网络连接崩溃时，系统保持 TCP/IP 连接。

## <a name="requirements"></a>要求

SMB 直通要求如下：

- 至少两台运行 Windows Server 2012 R2 或 Windows Server 2012 的计算机
- 一个或多个支持 RDMA 功能的网络适配器

### <a name="considerations-when-using-smb-direct"></a>使用 SMB 直通时的注意事项

- 可以在故障转移集群中使用 SMB 直通；然而，对于 SMB 直通而言，这需要确保用于客户端访问的集群网络的强大性。 故障转移集群支持使用多个网络进行客户端访问，同时使用支持 RSS（接收方扩展技术）和 RDMA 功能的网络适配器。
- 在 Hyper-V 管理操作系统上，使用 SMB 直通来支持 Hyper-V 在 SMB 之上的使用，并在使用 Hyper-V 存储器堆栈的虚拟机上提供存储空间。 然而，支持 RDMA 功能的网络适配器却不能直接用于 Hyper-V 客户端。 如果将支持 RDMA 功能的网络适配器连接到虚拟交换机上，自交换机上的虚拟网络适配器将不再是支持 RDMA 功能的网络适配器。
- 如果禁用 SMB 多通道，也将同时禁用 SMB 直通。 由于 SMB 多通道用于检测网络适配器的功能，并确定网络适配器是否支持 RDMA，禁用 SMB 多通道之后，客户端将无法使用 SMB 直通。
- Windows RT 不支持 SMB 直通。 SMB Direct 需要支持 RDMA 功能的网络适配器，该适配器仅在 Windows Server 2012 R2 和 Windows Server 2012 上可用。
- Windows Server 低端版本不支持 SMB 直通。 它仅在 Windows Server 2012 R2 和 Windows Server 2012 上受支持。

## <a name="enabling-and-disabling-smb-direct"></a>启用和禁用 SMB 直通

如果安装了 Windows Server 2012 R2 或 Windows Server 2012，则默认情况下启用 SMB 直通。 SMB 客户端自动执行检测，并在确定相应配置后使用攀个网络连接。

### <a name="disable-smb-direct"></a>禁用 SMB 直通

在通常情况下，无需禁用 SMB 直通，然而，在运行下面一种 Windows PowerShell 脚本时，可以将其禁用。

要禁用特定接口的 RDMA，键入：

```PowerShell
Disable-NetAdapterRdma <name>
```

要禁用所有接口的 RDMA，键入：

```PowerShell
Set-NetOffloadGlobalSetting -NetworkDirect Disabled
```

在客户端或服务器上禁用 RDMA 时，系统将无法使用该功能。 *Network Direct*是适用于 RDMA 接口的 windows Server 2012 R2 和 windows server 2012 基本网络支持的内部名称。

### <a name="re-enable-smb-direct"></a>重新启用 SMB 直通

禁用 RDMA 之后，可以通过运行下面一个 Windows PowerShell 脚本重新启用该功能。

要重新启用特定接口的 RDMA，键入：

```PowerShell
Enable-NetAdapterRDMA <name>
```

要重新启用所有接口的 RDMA，键入：

```PowerShell
Set-NetOffloadGlobalSetting -NetworkDirect Enabled
```

RDMA 需要在客户端和服务器上同时启用后，方可再次使用。

## <a name="test-performance-of-smb-direct"></a>测试 SMB 直通的性能

通过使用下面一个程序，可以测试 SMB 直通的工作性能。

### <a name="compare-a-file-copy-with-and-without-using-smb-direct"></a>比较使用和不使用 SMB 直通进行的文件复制

下面介绍了如何衡量 SMB 直通的增加吞吐量：

1. 配置 SMB 直通
2. 测量使用 SMB 直通进行较大文件复制的时间量
3. 在网络适配器上禁用 RDMA，请参阅 [Enabling and disabling SMB Direct](#enabling-and-disabling-smb-direct)。
4. 测量不使用 SMB 直通进行较大文件复制的时间量
5. 重新启用网络适配器上的 RDMA，然后比较两个结果。
6. 为避免缓存影响，应执行下列操作：
    1. 复制大量数据（处理超出内存量的大量数据的功能）。
    2. 复制数据两次，第一次复制为操作，第二次复制为定时传输。
    3. 进行每次测试前重启服务器和客户端，以便确保它们在相似条件下运行。

### <a name="fail-one-of-multiple-network-adapters-during-a-file-copy-with-smb-direct"></a>使用 SMB 直通执行文件复制过程中，多个网络适配器中有一个发生故障

下面是如何确认 SMB 直通的故障转移功能：

1. 确保 SMB 直通在多个网络适配器配置环境下正确工作。
2. 运行较大文件复制。 运行复制过程中，通过断开一条电缆（或通过禁用一个网络适配器）来模拟网络路径发生的故障。
3. 确定使用其余一个网络适配器继续传输文件复制，而未发生任何文件复制错误。

>[!NOTE]
>在不使用 SMB 直通的情况下，为避免工作负载失败，确保在网络路径中断后不再执行其他工作负载。

## <a name="more-information"></a>详细信息

- [服务器消息块概述](file-server-smb-overview.md)
- @no__t 0Increasing 服务器、存储和网络可用性：方案概述](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831437(v%3dws.11)>)
- [部署基于 SMB 的 Hyper-v](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
