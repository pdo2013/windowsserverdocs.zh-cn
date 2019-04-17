---
title: 故障转移群集的硬件要求和存储选项
description: 硬件要求和用于创建故障转移群集的存储选项。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/26/2018
ms.localizationpriority: medium
ms.openlocfilehash: 4706372b06d0554196b692c3ddcda145dee5bae5
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/22/2018
ms.locfileid: "2081907"
---
# <a name="failover-clustering-hardware-requirements-and-storage-options"></a>故障转移群集的硬件要求和存储选项

适用于： Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2016

您需要创建故障转移群集的以下硬件。 必须由 Microsoft 支持，必须运行的的 Windows Server 的版本经过认证所有硬件，并完成故障转移群集解决方案必须通过验证配置向导中的所有测试。 有关验证故障转移群集的详细信息，请参阅[验证硬件故障转移群集](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>)。

- **服务器**： 建议使用一组的匹配包含相同或类似的与组件的计算机。
- **网络适配器和电缆 （用于网络通信）**： 如果使用 iSCSI，应为每个网络适配器专用于网络通信或 iSCSI，而不是同时。

    在将群集节点连接的网络基础结构，应避免使用单点故障。 例如，您可以通过多个连接群集节点不同网络。 或者，您可以与网络适配器组、 冗余交换机、 冗余路由器或类似删除单点故障的硬件与一个在构建的网络连接群集节点。

    >[!NOTE]
    >如果您使用单个网络连接群集节点，网络将验证配置向导中传递的冗余要求。 但是，从向导报告将包含网络不应包含单点故障的警告。

- **设备控制器或适当的适配器，用于存储**：

  - **串行附加 SCSI 或光纤通道**： 如果使用串行附加 SCSI 或光纤通道，在所有群集服务器，应相同存储堆栈中的所有元素。 需要多路径 I/O (MPIO) 软件是相同和设备特定模块 (DSM) 软件完全相同。 建议的大容量存储设备控制器 — 主机总线适配器 (HBA)、 HBA 驱动程序和 HBA 固件 — 附加到群集存储在相同。 如果您使用不同的 Hba，应验证存储供应商您遵循其支持的或建议的配置。
  - **iSCSI**： 如果使用 iSCSI，每个群集的服务器应具有一个或多个网络适配器或专用于群集存储的 Hba。 使用 iSCSI 网络不应用于网络通信。 在所有群集服务器网络适配器用于连接到 iSCSI 存储目标应该是相同的并且我们建议使用千兆以太网或更高版本。
- **存储**： 您必须使用[存储空格直接](../storage/storage-spaces/storage-spaces-direct-overview.md)或共享与 Windows Server 2012 R2 或 Windows Server 2012 兼容的存储。 您可以使用共享附加的存储和您还可以用作 SMB 3.0 文件共享共享存储运行 HYPER-V 在故障转移群集中配置的服务器。 有关详细信息，请参阅[部署 HYPER-V 通过 SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)。

    在大多数情况下，附加的存储应包含多个、 独立磁盘 （逻辑单元号或 Lun） 级别的硬件配置。 对于某些群集，一个磁盘作为磁盘见证 （在此子节的末尾所述）。 其他磁盘包含 （以前称为群集的服务或应用程序） 的群集角色所需的文件。 存储要求包括：

  - 若要使用包含在故障转移群集的本机磁盘支持，请使用基本磁盘，不动态磁盘。
  - 我们建议您格式化为 NTFS 分区。 如果您使用群集共享卷 (CSV)，其中每个分区必须 NTFS。

    >[!NOTE]
    >如果您有见证磁盘仲裁配置，您可以设置格式与 NTFS 或弹性文件系统 (ReFS) 磁盘。

  - 对于磁盘分区样式，您可以使用主启动记录 (MBR) 或 GUID 分区表 (GPT)。

    见证磁盘是群集存储的具有指定用于保存一份群集配置数据库中的磁盘。 仅当此值指定为仲裁配置的一部分，故障转移群集具有见证磁盘。 有关详细信息，请参阅[了解仲裁中存储空格直接](../storage/storage-spaces/understand-quorum.md)。

## <a name="hardware-requirements-for-hyper-v"></a>针对 HYPER-V 的硬件要求

如果您正在创建包括群集的虚拟机的故障转移群集，群集服务器必须支持 HYPER-V 角色的硬件要求。 HYPER-V 要求 64 位处理器，其中包括：

- 硬件辅助虚拟化。 这是包含虚拟化选项的处理器中提供，特别是具有 Intel 虚拟化技术 (Intel VT) 或 AMD 虚拟化 (AMD-V) 技术的处理器。
- 硬件实施数据执行保护 (DEP) 必须提供并启用。 具体而言，必须启用位 Intel XD （执行禁用位） 或 AMD NX 位 （没有执行位）。

有关 HYPER-V 角色的详细信息，请参阅[HYPER-V 概述](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831531(v%3dws.11)>)。

## <a name="deploying-storage-area-networks-with-failover-clusters"></a>部署与故障转移群集的存储区域网络

在部署故障转移群集与存储区域网络 (SAN)，请遵循以下准则：

- **确认的存储空间的兼容性**： 与制造商和供应商的存储，包括驱动程序、 固件和存储，所使用的软件版本中的正在运行的 Windows Server 故障转移群集与兼容的确认。
- **隔离存储设备，每个设备的一个群集**： 不同群集中的服务器必须能够访问相同的存储设备。 在大多数情况下，用于群集服务器的一组 LUN 应独立于通过 LUN 屏蔽或分区的所有其他服务器。
- **考虑使用多路径 I/O 软件或搭配使用网络适配器**： 在高可用性存储结构中，您可以使用多路径 I/O 软件部署具有多个主机总线适配器的故障转移群集或网络适配器结合使用 （也称为负载平衡和故障转移或 LBFO）。 此提供冗余和可用性的最高的级别。 对于 Windows Server 2012 R2 或 Windows Server 2012，多路径解决方案必须基于 Microsoft 多路径 I/O (MPIO)。 尽管 Windows Server 操作系统的一部分包含一个或多个 DSMs，硬件供应商通常将提供您的硬件，MPIO 设备特定模块 (DSM)。

    有关 LBFO 的详细信息，请参阅 Windows Server 技术库中的[NIC 结合使用概述 （英文)](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming) 。

    >[!IMPORTANT]
    >主机总线适配器和多路径 I/O 软件可以版本十分敏感。 如果要为群集实现多路径解决方案，与硬件供应商，以选择正确的适配器、 固件和正在运行的 Windows Server 的版本的软件紧密合作。

- **考虑使用存储空间**： 如果您计划部署串行附加的 SCSI (SAS) 群集存储配置使用存储空间，请[部署群集的存储空间](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>)参阅要求。

## <a name="more-information"></a>详细信息

- [故障转移群集](failover-clustering.md)
- [存储空间](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831739(v%3dws.11)>)
- [使用来宾群集以实现高可用性](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn440540(v%3dws.11)>)