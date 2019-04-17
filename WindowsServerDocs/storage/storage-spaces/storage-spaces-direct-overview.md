---
title: 存储空间直通概述
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: dongill
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 03/26/2019
ms.assetid: 8bd0d09a-0421-40a4-b752-40ecb5350ffd
description: 存储空间直通，使你能够使用内部存储的服务器群集到软件定义的存储解决方案的 Windows Server 的一项功能的概述。
ms.localizationpriority: medium
ms.openlocfilehash: d71e4fc404f760102a2b22f71bbeec680868e9b8
ms.sourcegitcommit: e558dda2774345e9ad17ff04b759f68c59d88139
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2019
ms.locfileid: "9262780"
---
# 存储空间直通概述

>适用于： Windows Server 2019、 Windows Server 2016

存储空间直通使用具有本地连接驱动器的行业标准服务器来创建高度可用、高度可扩展的软件定义存储，其成本仅占传统 SAN 或 NAS 阵列的一小部分。 其聚合或超聚合体系结构大大简化了采购和部署，同时功能如缓存、 存储层和擦除编码，如 RDMA 网络和 NVMe 驱动器的最新的硬件创新，与提供无可匹敌的效率和性能。

存储空间直通包含在 Windows Server 2019 Datacenter、 Windows Server 2016 Datacenter，和[Windows Server Insider Preview 版本](https://insider.windows.com/for-business-getting-started-server/)。 

存储空间，其他应用程序，例如共享 SAS 群集和独立服务器，请参阅[存储空间概述](overview.md)。 如果你正在寻找有关在 Windows 10 电脑上使用存储空间的信息，请参阅[Windows 10 中的存储空间](https://support.microsoft.com/help/12438/windows-10-storage-spaces)。

<table>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>了解</a></strong>
            <ul>
              <li>概述（你目前位于此位置）</li>
              <li><a href="understand-the-cache.md">了解缓存</a></li>
              <li><a href="storage-spaces-fault-tolerance.md">容错和存储效率</a></li>
              <li><a href="drive-symmetry-considerations.md">驱动器对称注意事项</a></li>
              <li><a href="understand-storage-resync.md">了解并监视存储重新同步</a></li>
              <li><a href="understand-quorum.md">了解群集和池仲裁</a></li>
              <li><a href="cluster-sets.md">群集集</a></li>
            </ul>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>计划</a></strong>
            <ul>
              <li><a href="storage-spaces-direct-hardware-requirements.md">硬件要求</a></li>
              <li><a href="csv-cache.md">使用 CSV 内存中读取缓存</li>
              <li><a href="choosing-drives.md">选择驱动器</a></li>
              <li><a href="plan-volumes.md">规划卷</a></li>
              <li><a href="storage-spaces-direct-in-vm.md">使用来宾虚拟机群集</a></li>
              <li><a href="storage-spaces-direct-disaster-recovery.md">灾难恢复</a></li>
            </ul>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>部署</a></strong>
            <ul>
                    <li><a href="deploy-storage-spaces-direct.md">部署存储空间直通</a></li>
                    <li><a href="create-volumes.md">创建卷</a></li>
              <li><a href="nested-resiliency.md">嵌套复原</a></li>
              <li><a href="../../failover-clustering/manage-cluster-quorum.md">配置仲裁</a></li>
              <li><a href="upgrade-storage-spaces-direct-to-windows-server-2019.md">将存储空间直通群集升级为 Windows Server 2019</a></li>
            </ul>
        </td>        
        <td style="padding: 5px; border: 0;">
            <strong>管理</a></strong>
            <ul>
              <li><a href="../../manage/windows-admin-center/use/manage-hyper-converged.md">使用 Windows Admin Center 管理</a></li>
              <li><a href="add-nodes.md">添加服务器或驱动器</a></li>
              <li><a href="maintain-servers.md">使服务器脱机以进行维护</li>
              <li><a href="remove-servers.md">删除服务器</a></li>
              <li><a href="resize-volumes.md">扩展卷</a></li>
              <li><a href="../update-firmware.md">更新驱动器固件</a></li>
              <li><a href="performance-history.md">性能历史记录</a></li>
              <li><a href="delimit-volume-allocation.md">分隔卷的分配</a></li>
              <li><a href="configure-azure-monitor.md">超聚合群集上使用 Azure 监视器</a></li>
            </ul>
        </td>
    </tr>
    <tr style="border: 0;">
         <td style="padding: 5px; border: 0;">
            <strong>疑难解答</a></strong>
            <ul>
              <li><a href="storage-spaces-states.md">运行状况和操作状态疑难解答</a></li>
              <li><a href="data-collection.md">使用存储空间直通收集诊断数据</a></li>
            </ul>
         <td style="padding: 5px; border: 0;">
            <strong>最近的博客文章</a></strong>
            <ul>
              <li><a href="https://blogs.technet.microsoft.com/filecab/2018/10/30/windows-server-2019-and-intel-optane-dc-persistent-memory/">使用存储空间直通 13.7 包含一百万个 IOPS： 超级集成基础架构的新行业记录</a></li>
              <li><a href="https://blogs.technet.microsoft.com/filecab/2018/10/02/hci-the-countdown-clock-starts-now/">Windows Server 2019 的超聚合基础结构的倒计时时钟现在启动 ！</a></li>
              <li><a href="https://blogs.technet.microsoft.com/filecab/2018/06/27/windows-server-summit-recap/">从 Windows Server 会议的五个大公告</a></li>
              <li><a href="https://blogs.technet.microsoft.com/filecab/2018/03/27/storage-spaces-direct-momentum/">10000 存储空间直通群集和计数...</a></li>
            </ul>
</table>

## 视频

**快速视频概述 （5 分钟）**

<iframe src="https://www.youtube-nocookie.com/embed/raeUiNtMk0E" width="560" height="315" allowfullscreen></iframe>

**存储空间直通在 Microsoft Ignite 2018 （1 小时）**

[在 YouTube 上观看](https://www.youtube.com/watch?v=5kaUiW3qo30)

**存储空间直通 Microsoft Ignite 2017 （1 小时）**

[在 YouTube 上观看](https://www.youtube.com/watch?v=YDr2sqNB-3c)

**启动 Microsoft Ignite 2016 （1 小时） 上的事件**

[在 YouTube 上观看](https://www.youtube.com/watch?v=-LK2ViRGbWs)

## 主要优势

<table>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/simplicity-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>简单易用。</b> 在 15 分钟内即可从运行 Windows Server 2016 的行业标准服务器转到第一个存储空间直通群集。 对于 System Center 用户而言，部署不仅仅只是一个复选框。
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/performance-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>无可匹敌的性能。</b> 无论是全闪存还是混合存储空间直通，都可以轻松超越<a href="https://blogs.technet.microsoft.com/filecab/2016/07/26/storage-iops-update-with-storage-spaces-direct/">每台服务器 150,000 次混合 4K 随机 IOPS</a> 的限制，并且具有较高的一致性和较低的延迟，所有这一切均归功于其嵌入虚拟机监控程序的体系结构、内置的读/写缓存，以及对直接安装在 PCIe 总线上的尖端 NVMe 驱动器的支持。
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/fault-tolerance-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>容错。 </b> 内置的复原功能可在不影响可用性的情况下处理驱动器、服务器或组件故障。 大规模部署还可以配置<a href="../../failover-clustering/fault-domains.md">机箱和机架容错</a>。 如果硬件发生故障，只需更换发生故障的硬件，而软件会自行复原，不需要复杂的管理步骤。
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/efficiency-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>资源效率。</b> 擦除编码最多可提高 2.4 倍的存储效率，它采用独特的创新（如本地重建代码和 ReFS 实时层），使效率优势扩展到了硬盘驱动器及热/冷混合工作负载，同时最大程度降低了 CPU 使用率，为最需要的设备（即 VM）提供资源。
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/manageability-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>可管理性。</b> 使用<a href="../storage-qos/storage-qos-overview.md">存储 QoS 控件</a>，使过于繁忙的 VM 保持符合每台 VM 的 IOPS 上限和下限。 <a href="../../failover-clustering/health-service-overview.md">运行状况服务</a>提供连续的内置监视和警报，并使用新的 API，可在群集范围内轻松收集大量性能和容量指标。
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/scalability-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>可扩展性。</b> 可扩展到 16 台服务器以及超过 400 个驱动器，每个群集拥有最高 1 PB (1,000 TB) 的存储。 只需添加驱动器或添加更多服务器即可实现扩展，存储空间直通会自动载入新的驱动器，并开始使用它们。 存储效率和性能如预期大幅提升。
        </td>
    </tr>
</table>

## 部署选项

存储空间直通适用于两种不同的部署选项：

### 聚合

**单独群集中的存储和计算。** 聚合部署选项（也称为“分解部署”）将横向扩展文件服务器 (SoFS) 放在存储空间直通之上，通过 SMB3 文件共享提供网络连接存储。 它可以独立于存储群集扩展计算/工作负荷，服务提供商和企业的 Hyper-V IaaS（服务架构）等大规模部署就需要采用这种部署。

![存储空间直通使用横向扩展文件服务器功能为另一个服务器或群集中的 Hyper-V VM 提供存储](media/storage-spaces-direct-in-windows-server-2016/converged-minimal.png)

### 超聚合

**一个用于计算和存储的群集。** 超聚合部署选项直接在提供存储、在本地卷上存储文件的服务器上运行 Hyper-V 虚拟机或 SQL Server 数据库。 此部署选项无需配置文件服务访问和权限，降低了中小型企业或远程机构/分支机构部署的硬件成本。 请参阅[部署存储空间直通](deploy-storage-spaces-direct.md)。

![存储空间直通为同一群集中的 Hyper-V VM 提供存储](media/storage-spaces-direct-in-windows-server-2016/hyper-converged-minimal.png)

## 工作原理

存储空间直通由存储空间演变而来，最先在 Windows Server 2012 中引入。 它利用 Windows Server 中当前已知的许多功能，如故障转移群集、群集共享卷 (CSV) 文件系统、服务器消息块 (SMB) 3 以及存储空间。 它还引入了新技术，最值得注意的是软件存储总线。

下面部分概括介绍存储空间直通堆栈：

![存储空间直通堆栈](media/storage-spaces-direct-in-windows-server-2016/converged-full-stack.png)

**网络硬件。** 存储空间直通使用 SMB3（包括 SMB 直通和 SMB 多通道）通过以太网在服务器之间通信。 We strongly recommend 10+ GbE with remote-direct memory access (RDMA), either iWARP or RoCE.

**存储硬件。** 2-16 台具有本地连接 SATA、SAS 或 NVMe 驱动器的服务器。 每台服务器至少必须有 2 个固态驱动器，至少 4 个其他驱动器。 SATA 和 SAS 设备应位于主机总线适配器 (HBA) 和 SAS 扩展器之后。 强烈建议使用合作伙伴提供的平台，这种平台经过精心设计并广泛验证，即将推出。

**故障转移群集。** Windows Server 的内置群集功能用于连接服务器。

**软件存储总线。** 存储空间直通中新增了软件存储总线。 它可以跨越群集，建立软件定义的存储构造，在这种构造中，所有服务器都可以相互看到对方的所有本地驱动器。 可以考虑使用它来代替成本高昂且局限的光纤通道或共享 SAS 电缆。

**存储总线层缓存。** 软件存储总线将目前最快的驱动器（例如 SSD）动态绑定到较慢的驱动器（例如 HDD），以提供服务器端读/写缓存，从而加速 IO 并提高吞吐量。

**存储池。** 形成存储空间基础的驱动器集合称为存储池。 存储池是自动创建的，它自动发现符合条件的所有驱动器，并将这些驱动器添加到池中。 强烈建议一个群集使用一个池，并采用默认设置。 请阅读我们的[深入探讨存储池](https://blogs.technet.microsoft.com/filecab/2016/11/21/deep-dive-pool-in-spaces-direct/)，了解详细信息。

**存储空间。** 存储空间通过[镜像和/或擦除编码](storage-spaces-fault-tolerance.md)对虚拟“磁盘”提供容错功能。 可以将其视为软件定义的分布式 RAID，使用的是池中的驱动器。 在存储空间直通中，除了机箱和机架容错外，这些虚拟磁盘通常还具有复原功能，可以复原两个同时发生的驱动器或服务器故障（例如，3 向镜像，各数据副本存储在不同的服务器中）。

**复原文件系统 (ReFS)。** ReFS 是专门针对虚拟化构建的首要文件系统。 它可以显著加快 .vhdx 文件的操作速度（如创建、扩展以及检查点合并），并内置了校验和功能，可检测并纠正位错误。 它还引入了实时层，可以根据使用情况在所谓的“热”、“冷”存储层之间实时轮换数据。

**群集共享卷。** CSV 文件系统将所有 ReFS 卷整合为一个命名空间，可以通过任何服务器进行访问，对于每台服务器而言，每个卷的运行就像是本地安装在每台服务器上一样。

**横向扩展文件服务器。** 这是最后一层，仅在聚合部署中才需要。 它提供远程文件访问功能，可使用 SMB3 访问协议通过网络访问客户端（如运行 Hyper-V 的另一个群集），有效地将存储空间直通变为网络连接存储 (NAS)。

## 客户案例

有[超过 10000 群集](https://blogs.technet.microsoft.com/filecab/2018/03/27/storage-spaces-direct-momentum/)范围内运行的存储空间直通。 规模的组织所有，从小型企业部署到大型企业和政府部署数百个节点，只需两个节点，取决于存储空间直通其关键应用程序和基础结构。

请访问[Microsoft.com/HCI](https://www.microsoft.com/hci)读取其情景：

[![G删除客户徽标](media/storage-spaces-direct-in-windows-server-2016/customer-stories.png)](https://www.microsoft.com/hci)

## 管理工具

可以使用以下工具管理和/或监视存储空间直通：

| 姓名 | 图形或命令行？ | 付费或包含？ |
|-----------------|----------------------------|-------------------|
| [Windows Admin Center](../../manage/windows-admin-center/overview.md)     | 图形    | 已包含 |
| 服务器管理器 & 故障转移群集管理器                                 | 图形    | 已包含 |
| Windows PowerShell                                                        | 命令行 | 已包含 |
| [System Center Virtual Machine Manager (SCVMM)](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-storage-spaces-direct-vmm) & [Operations Manager (SCOM)](https://www.microsoft.com/download/details.aspx?id=54700) | 图形    | Paid     |

## 入门

[在 Microsoft Azure 中](https://blogs.technet.microsoft.com/filecab/2016/05/05/s2dazuretp5/)试用存储空间直通，或者从 [Windows Server 评估](https://go.microsoft.com/fwlink/?linkid=842602) 中下载 WindowsServer 的 180 天许可证评估副本。

## 另请参阅

-   [容错和存储效率](storage-spaces-fault-tolerance.md)
-   [存储副本](../storage-replica/storage-replica-overview.md)
-   [存储在 Microsoft 博客](https://blogs.technet.microsoft.com/filecab/)
-   [具有 iWARP 的存储空间直通吞吐量](https://blogs.technet.microsoft.com/filecab/2017/03/13/storage-spaces-direct-throughput-with-iwarp)（TechNet 博客）
-   [Windows Server 中的故障转移群集的新增功能](../../failover-clustering/whats-new-in-failover-clustering.md)  
-   [存储服务质量](../storage-qos/storage-qos-overview.md)
-   [Windows IT 专业人员支持](https://www.microsoft.com/itpro/windows/support)
