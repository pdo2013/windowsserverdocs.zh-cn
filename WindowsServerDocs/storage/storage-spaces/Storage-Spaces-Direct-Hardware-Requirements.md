---
title: 存储空间直通的硬件要求
ms.prod: windows-server-threshold
description: 测试存储空间直通的最低硬件要求。
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 06/13/2019
ms.localizationpriority: medium
ms.openlocfilehash: 7fa4560e0c050c8decbcb4e9456a884976e447e2
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284421"
---
# <a name="storage-spaces-direct-hardware-requirements"></a>存储空间直通的硬件要求

> 适用于：Windows Server 2019、Windows Server 2016

本主题介绍存储空间直通的最低硬件要求。

对于生产环境，Microsoft 建议在我们的合作伙伴，包括部署工具和过程购买已验证的硬件/软件解决方案。 这些解决方案设计、 组装和根据我们的参考体系结构，以确保兼容性和可靠性，因此你和快速运行验证。 对于 Windows Server 2019 解决方案，请访问[Azure Stack HCI solutions 网站](https://azure.microsoft.com/overview/azure-stack/hci)。 对于 Windows Server 2016 的解决方案，了解详细信息，请[Windows Server 软件定义](https://microsoft.com/wssd)。

   > [!TIP]
   > 想要评估存储空间直通，但没有硬件？ 使用 HYPER-V 或 Azure 虚拟机中所述[使用存储空间直通中来宾虚拟机群集](storage-spaces-direct-in-vm.md)。

## <a name="base-requirements"></a>基本要求

系统、 组件、 设备和驱动程序必须是**认证的 Windows Server 2016**每个[Windows Server 目录](https://www.windowsservercatalog.com)。 此外，我们建议服务器、 驱动器、 主机总线适配器和网络适配器具有**软件定义数据中心 (SDDC) 标准**和/或**软件定义数据中心 (SDDC) 高级**附加资格 (AQs)，如如下图所示。 有 SDDC AQs 具有超过 1000 个组件。

![显示 SDDC AQs 在 Windows Server 目录的屏幕截图](media/hardware-requirements/sddc-aqs.png)

完全配置的群集 （服务器、 网络和存储） 必须通过所有[群集验证测试](https://technet.microsoft.com/library/cc732035(v=ws.10).aspx)每个故障转移群集管理器中或使用该向导`Test-Cluster` [cmdlet](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps)在 PowerShell 中。

此外，满足以下要求：

## <a name="servers"></a>服务器

- 最少 2 台服务器，最多 16 台服务器
- 建议所有服务器的相同制造商和型号

## <a name="cpu"></a>CPU

- 以及 Intel Nehalem 或更高版本的兼容处理器;或
- AMD EPYC 或更高版本的兼容处理器

## <a name="memory"></a>内存

- Windows Server、 Vm 和其他应用程序或工作负荷; 的内存plus
- 4 GB 的 RAM 每 tb 的存储空间直通的元数据的每台服务器上的缓存驱动器容量

## <a name="boot"></a>启动

- Windows Server 支持的任何启动设备的[现在包括 SATADOM](https://cloudblogs.microsoft.com/windowsserver/2017/08/30/announcing-support-for-satadom-boot-drives-in-windows-server-2016/)
- RAID 1 镜像已**不**必需的但启动支持
- 建议：200 GB 的最小大小

## <a name="networking"></a>网络

最小值 （适用于小规模 2-3 个节点）
- 10 Gbps 网络接口
- 2 节点直接连接 （无交换机） 支持

建议 （适用于高性能、 规模或 4 及更多节点部署在）
- Nic 的远程直接内存访问 (RDMA) 支持，（推荐） iWARP 或 RoCE
- 针对冗余性和性能的两个或多个 Nic
- 25 Gbps 网络接口或更高版本

## <a name="drives"></a>驱动器

存储空间直通适用于直接连接 SATA、 SAS 或 NVMe 驱动器的物理连接到只需一台服务器。 有关选择驱动器的更多帮助，请参阅[选择驱动器](choosing-drives.md)主题。

- 都被支持 SATA、 SAS 和 NVMe （M.2、 U.2，并添加在卡） 驱动器
- 支持 512n、 512e 和 4k 本机驱动器
- 固态驱动器必须提供[电源中断保护](https://blogs.technet.microsoft.com/filecab/2016/11/18/dont-do-it-consumer-ssd/)
- 相同数量和类型的驱动器中的每个服务器 – 请参阅[驱动器对称性注意事项](drive-symmetry-considerations.md)
- 缓存设备必须是 32 GB 或更大
- 永久性内存设备缓存设备时，必须使用 NVMe 或 SSD 容量设备 （不能使用 Hdd）
- NVMe 驱动程序是 Microsoft 的内置或更新 NVMe 驱动程序。
- 建议：容量驱动器的数量是整数倍的缓存驱动器数
- 建议：缓存驱动器应具有较高的写入耐用性： 至少 3 个驱动器的写入-每天 (DWPD) 或至少为 4 千吉字节 (TBW) 编写每日 – 请参阅[了解驱动器兆兆字节写入 (TBW) 和所需的最低建议用于存储每一天 (DWPD) 写入空间直通](https://blogs.technet.microsoft.com/filecab/2017/08/11/understanding-dwpd-tbw/)

下面是可以连接的存储空间直通的驱动器的方式：

- 直接连接 SATA 驱动器
- 直连 NVMe 驱动器
- SAS 驱动器的 SAS 主机总线适配器 (HBA)
- SATA 驱动器的 SAS 主机总线适配器 (HBA)
- **不支持：** RAID 控制器卡或 SAN （光纤通道、 iSCSI、 FCoE） 存储。 主机总线适配器 (HBA) 卡必须实现简单的传递模式。

![互连设备的受支持的驱动器的关系图](media/hardware-requirements/drive-interconnect-support-1.png)

驱动器可以在服务器的内部或外部机箱中这是到只剩一个连接的服务器。 SCSI 机箱服务 (SES) 是必需的槽映射和标识。 每个外部机箱必须提供唯一标识符 (唯一 ID)。

- 服务器的内部驱动器
- 驱动器连接到一个服务器到外部机器 ("JBOD")
- **不支持：** 共享的 SAS 存储设备连接到多个服务器或任何形式的多路径 IO (MPIO) 驱动器是可由多个路径访问。

![互连设备的受支持的驱动器的关系图](media/hardware-requirements/drive-interconnect-support-2.png)

### <a name="minimum-number-of-drives-excludes-boot-drive"></a>驱动器的最小数目 （不包括启动驱动器）

- 如果有用作缓存的驱动器，则每个服务器必须至少有 2 个驱动器
- 每个服务器必须至少有 4 个容量（非缓存）驱动器

| 存在驱动器类型   | 所需的最小数量 |
|-----------------------|-------------------------|
| 所有永久性内存 （相同的模型） | 4 永久性内存 |
| 所有 NVMe（同一型号） | 4 个 NVMe                  |
| 所有 SSD（同一型号）  | 4 个 SSD                   |
| 永久性内存 + NVMe 或 SSD | 2 个持久的内存 + 4 NVMe 或 SSD |
| NVMe + SSD            | 2 个 NVMe + 4 个 SSD          |
| NVMe + HDD            | 2 个 NVMe + 4 个 HDD          |
| SSD + HDD             | 2 个 SSD + 4 个 HDD           |
| NVMe + SSD + HDD      | 2 个 NVMe + 4 个其他       |

   >[!NOTE]
   > 此表提供有关硬件部署的最小值。 如果已部署的虚拟机和虚拟化存储，例如 Microsoft Azure，请参阅[使用存储空间直通中来宾虚拟机群集](storage-spaces-direct-in-vm.md)。

### <a name="maximum-capacity"></a>最大容量

| 最大值                | Windows Server 2019  | Windows Server 2016  |
| ---                     | ---------            | ---------            |
| 每个服务器的原始容量 | 100 TB               | 100 TB               |
| 池容量           | 4 PB (4,000 TB)      | 1 PB                 |
