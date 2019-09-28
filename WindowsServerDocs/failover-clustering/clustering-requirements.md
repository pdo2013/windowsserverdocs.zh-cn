---
title: 故障转移群集硬件要求和存储选项
description: 用于创建故障转移群集的硬件要求和存储选项。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/26/2018
ms.localizationpriority: medium
ms.openlocfilehash: ebd4e50d712b834db1f0fd7f8e46d27697a4065f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361221"
---
# <a name="failover-clustering-hardware-requirements-and-storage-options"></a>故障转移群集硬件要求和存储选项

适用于：Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

若要创建故障转移群集，你需要以下硬件。 若要获得 Microsoft 的支持，所有硬件必须针对正在运行的 Windows Server 的版本进行了认证，并且完整的故障转移群集解决方案必须通过“验证配置向导”中的所有测试。 有关验证故障转移群集的详细信息，请参阅[验证故障转移群集的硬件](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>)。

- **服务器**：建议使用一组包含相同或相似组件的匹配计算机。
- **网络适配器和电缆（用于网络通信）** ：如果使用 iSCSI，则应将每个网络适配器专用于网络通信或 iSCSI，而不能同时用于这两者。

    在连接群集节点的网络基础结构中，应避免出现单点故障。 例如，你可以通过多个不同的网络来连接群集节点。 或者，你可以将群集节点与通过组合网络适配器、冗余交换机、冗余路由器或可消除单故障点的类似硬件一起构造的一个网络连接在一起。

    >[!NOTE]
    >如果使用一个网络来连接群集节点，该网络将需要符合“验证配置向导”中的冗余要求。 但是，此向导生成的报告将包含一条有关该网络不应具有单点故障的警告。

- **用于存储的设备控制器或相应适配器**：

  - **串行连接 SCSI 或光纤通道**：如果你在所有群集服务器中使用串行连接 SCSI 或光纤通道，则存储堆栈的所有组件都应相同。 需要多路径 i/o （MPIO）软件相同，并且设备特定模块（DSM）软件是相同的。 建议将大容量存储设备控制器（即主机总线适配器（HBA）、HBA 驱动程序以及 HBA 固件）附加到群集存储。 如果使用不同的 HBA，则应向存储供应商验证你是否采用了其支持或推荐的配置。
  - **iSCSI**：如果使用的是 iSCSI，则各群集服务器都应具有一个或多个专用于群集存储的网络适配器或 HBA。 不应将用于 iSCSI 的网络用于网络通信。 在所有群集服务器中，用于连接 iSCSI 存储目标的网络适配器都应相同，建议使用千兆或更高速度的以太网。
- **存储**：必须使用与 Windows Server 2012 R2 或 Windows Server 2012 兼容的[存储空间直通](../storage/storage-spaces/storage-spaces-direct-overview.md)或共享存储。 你可以使用附加的共享存储，还可以将 SMB 3.0 文件共享用作在故障转移群集中配置的运行 Hyper-v 的服务器的共享存储。 有关详细信息，请参阅 [部署基于 SMB 的 Hyper-V](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)。

    在大多数情况下，附加的存储应包含在硬件级别上配置的多个独立磁盘（逻辑单元号或 LUN）。 某些群集使用一个磁盘作为磁盘见证（在此子部分末尾进行介绍）。 其他磁盘包含群集角色（以前称为群集服务或应用程序）所需的文件。 存储要求如下：

  - 若要使用故障转移群集中包含的本机磁盘支持，请使用基本磁盘（而非动态磁盘）。
  - 建议使用 NTFS 格式化分区。 如果使用群集共享卷 (CSV)，则其中每个分区都必须采用 NTFS 格式。

    >[!NOTE]
    >如果你有见证磁盘仲裁配置，可以使用 NTFS 或弹性文件系统 (ReFS) 格式化磁盘。

  - 对于磁盘分区形式，可使用主启动记录 (MBR) 或 GUID 分区表 (GPT)。

    磁盘见证是群集存储中的一个磁盘，它被指定用于保存群集配置数据库的副本。 仅当它指定为仲裁配置的一部分时，故障转移群集才具有磁盘见证。 有关详细信息，请参阅[了解存储空间直通中的仲裁](../storage/storage-spaces/understand-quorum.md)。

## <a name="hardware-requirements-for-hyper-v"></a>Hyper-V 的硬件要求

如果要创建包括群集虚拟机的故障转移群集，则群集服务器必须支持 Hyper-V 角色的硬件要求。 Hyper-V 需要一个 64 位处理器，包括以下要求：

- 硬件协助的虚拟化。 在提供虚拟化选项的处理器上，可以进行硬件协助的虚拟化 — 特别是具有 Intel 虚拟化技术  (Intel VT) 或 AMD 虚拟化 (AMD-V) 技术的处理器。
- 硬件强制实施的数据执行保护 (DEP) 必须可用且已启用。 具体地说就是，你必须启用 Intel XD 位（执行禁用位）或 AMD NX 位（无执行位）。

有关 Hyper-V 的详细信息，请参阅 [Hyper-V 概述](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831531(v%3dws.11)>)。

## <a name="deploying-storage-area-networks-with-failover-clusters"></a>使用故障转移群集部署存储区域网络

使用故障转移群集部署存储区域网络 (SAN) 时，请遵循以下准则：

- **确认存储的兼容性**：请与制造商和供应商确认存储（包括存储所使用的驱动程序、固件和软件）是否与运行的 Windows Server 版本中的故障转移群集兼容。
- **隔离存储设备，每个设备一个群集**：来自不同群集的服务器不能访问同一存储设备。 多数情况下，用于一组群集服务器的一个 LUN 不应通过 LUN 屏蔽或分区而与所有其他服务器隔离。
- **考虑使用多路径 I/O 软件或网络适配器组**：在高度可用的存储构造中，你可以使用多路径 I/O 软件或网络适配器组（也称为负载平衡和故障转移或 LBFO）部署具有多个主机总线适配器的故障转移群集。 这可以提供最高级别的冗余和可用性。 对于 Windows Server 2012 R2 或 Windows Server 2012，多路径解决方案必须基于 Microsoft 多路径 i/o （MPIO）。 虽然 Windows Server 包含一个或多个设备特定模块 (DSM) 作为操作系统的一部分，但你的硬件供应商通常会为你的硬件提供一个 MPIO DSM。

    有关 LBFO 的详细信息，请参阅 Windows Server 技术库中的[NIC 组合概述](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming)。

    >[!IMPORTANT]
    >主机总线适配器和多路径 I/O 软件可能对版本非常敏感。 如果你要对群集实现一个多路径解决方案，则应同你的硬件供应商密切协作，以便为运行的 Windows Server 版本选择正确的适配器、固件和软件。

- **考虑使用存储空间**：如果你计划部署使用存储空间配置的串行连接 SCSI （SAS）群集存储，请参阅为要求[部署群集存储空间](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>)。

## <a name="more-information"></a>详细信息

- [故障转移群集](failover-clustering.md)
- [存储空间](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831739(v%3dws.11)>)
- [使用来宾群集实现高可用性](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn440540(v%3dws.11)>)