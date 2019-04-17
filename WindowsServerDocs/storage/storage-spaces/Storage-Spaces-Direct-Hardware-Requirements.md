---
title: 存储空间直通的硬件要求
ms.prod: windows-server-threshold
description: 测试存储空间直通的最低硬件要求。
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 04/12/2018
ms.localizationpriority: medium
ms.openlocfilehash: 84d10ab3e25500720dd13e2ba057dc3c5bf05a6f
ms.sourcegitcommit: 515b4fd5c40fcbae0e12a2c30090384533972353
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2018
ms.locfileid: "8232526"
---
# 存储空间直通的硬件要求

> 适用于： Windows Server 2016、 Windows Server Insider Preview

本主题介绍存储空间直通的最低硬件要求。

用于生产，Microsoft 建议我们的合作伙伴，包括部署工具和过程从这些[Windows Server 软件定义](https://microsoft.com/wssd)硬件/软件产品/服务。 它们设计、 组装和验证对我们的参考体系结构，以确保兼容性和可靠性，以便获取和快速运行。 了解更多信息，请访问[https://microsoft.com/wssd](https://microsoft.com/wssd)。

![Windows Server 软件定义合作伙伴的徽标](media/hardware-requirements/wssd-partners.png)

   > [!TIP]
   > 想要评估存储空间直通，但不具有硬件？ 使用 HYPER-V 或 Azure 虚拟机中[使用存储空间直通来宾虚拟机群集中](storage-spaces-direct-in-vm.md)所述。

## 基本要求

系统、 组件、 设备和驱动程序必须为每个[Windows Server 目录](https://www.windowsservercatalog.com)的**Windows Server 2016 认证**。 此外，我们建议服务器、 驱动器、 主机总线适配器和网络适配器有**Software-Defined 数据中心 (SDDC) 标准**和/或**Software-Defined 数据中心 (SDDC) 高级版**的其他资格 (AQs)，如下图所示下面。 有超过 1000 个组件，并且 SDDC AQs。

![显示 SDDC AQs 在 Windows Server 目录的屏幕截图](media/hardware-requirements/sddc-aqs.png)

完全配置的群集 （服务器、 网络和存储） 必须通过根据向导的所有[群集验证测试](https://technet.microsoft.com/library/cc732035(v=ws.10).aspx)故障转移群集管理器中或使用`Test-Cluster`powershell [cmdlet](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps) 。

此外，满足以下要求：

## 服务器

- 最少 2 台服务器，最多 16 台服务器
- 建议所有服务器都具有相同的制造商和型号

## CPU

- Intel Nehalem 或更高版本的兼容处理器;或
- AMD EPYC 或更高版本的兼容处理器

## 内存

- Windows Server、 Vm，并且其他应用或工作负荷; 内存plus
- 4 GB RAM 每 tb 缓存驱动器容量的每个服务器，存储空间直通元数据

## 启动

- 支持的 Windows Server，[现在包含 SATADOM](https://cloudblogs.microsoft.com/windowsserver/2017/08/30/announcing-support-for-satadom-boot-drives-in-windows-server-2016/)的任何启动设备
- RAID 1 镜像是**不**是必需的但启动支持
- 建议： 200 GB 最小大小

## 网络

最小值 （小范围 2-3 节点）
- 10 Gbps 的网络接口
- 2 节点直接连接的 （无交换机） 支持

建议 （对于高性能、 比例或部署的 4 个以上节点）
- NICs that are remote-direct memory access (RDMA) capable, iWARP (recommended) or RoCE
- 两个或多个 Nic 用于冗余和提升性能
- 25 Gbps 的网络接口或更高版本

## 驱动器

存储空间直通配合使用直接连接 SATA、 SAS 或 NVMe 驱动器以物理方式附加到只有一个服务器。 有关选择驱动器的更多帮助，请参阅[选择驱动器](choosing-drives.md)主题。

- SATA、 SAS 和 NVMe （M.2、 U.2，并添加在卡） 驱动器均受支持
- 512n、 512e 和 4k 本机驱动器均受支持
- 固态硬盘必须提供[断电保护](https://blogs.technet.microsoft.com/filecab/2016/11/18/dont-do-it-consumer-ssd/)
- 相同的数量和类型的驱动器中每个服务器 – 请参阅[驱动器对称注意事项](drive-symmetry-considerations.md)
- NVMe 驱动程序是 Microsoft 的收件箱或更新的 NVMe 驱动程序。
- 建议： 容量驱动器数量是缓存驱动器数量的整数倍
- 建议： 缓存驱动器应具有较高的写入耐用性： 至少为 3 驱动器的每日写入 (次数 DWPD) 或至少 4 兆兆字节数每日 – 写入 (TBW)，请参阅[了解每日驱动器写入次数 (DWPD)、 兆兆字节写入 (TBW) 和所需的最低推荐用于存储空间直通](https://blogs.technet.microsoft.com/filecab/2017/08/11/understanding-dwpd-tbw/)

下面是可以连接存储空间直通的驱动器的方式：

1. 直接连接 SATA 驱动器
2. 直接附加 NVMe 驱动器
3. 具有 SAS 驱动器的 SAS 主机总线适配器 (HBA)
4. SATA 驱动器的 SAS 主机总线适配器 (HBA)
5. **不受支持：** RAID 控制器卡或 SAN （光纤通道、 iSCSI、 FCoE） 存储。 主机总线适配器 (HBA) 必须实现简单传递模式。

![受支持的驱动器的图示互连](media/hardware-requirements/drive-interconnect-support-1.png)

驱动器可以是内部服务器，或在外部机箱连接到只有一个服务器。 需要为插槽映射和标识 SCSI Enclosure Services (SES)。 每个外部机箱必须存在唯一标识符 (唯一 ID)。

1. 内部服务器的驱动器
2. Drives in an external enclosure ("JBOD") connected to one server
3. **不受支持：** 共享的 SAS 机箱连接到多个服务器或任何形式的多路径 IO (MPIO) 其中驱动器可通过多个路径。

![受支持的驱动器的图示互连](media/hardware-requirements/drive-interconnect-support-2.png)

### 最小驱动器数 （不包括启动驱动器）

- 如果有用作缓存的驱动器，则每个服务器必须至少有 2 个驱动器
- 每个服务器必须至少有 4 个容量（非缓存）驱动器

| 存在驱动器类型   | 所需的最小数量 |
|-----------------------|-------------------------|
| 所有 NVMe（同一型号） | 4 个 NVMe                  |
| 所有 SSD（同一型号）  | 4 个 SSD                   |
| NVMe + SSD            | 2 个 NVMe + 4 个 SSD          |
| NVMe + HDD            | 2 个 NVMe + 4 个 HDD          |
| SSD + HDD             | 2 个 SSD + 4 个 HDD           |
| NVMe + SSD + HDD      | 2 个 NVMe + 4 个其他       |

   >[!NOTE]
   > 此表提供所需的最低硬件部署。 如果你正在部署与虚拟机和虚拟存储，如化 Microsoft Azure 中看到[使用存储空间直通来宾虚拟机群集中](storage-spaces-direct-in-vm.md)。

### 最大容量

- 建议： 每个服务器最多 100 千吉字节 (TB) 原始存储容量
- 最大 1 petabyte (1000 TB) 原始容量存储池中
