---
title: 存储迁移服务方面的常见问题 (FAQ)
description: 有关常见问题存储迁移服务，例如从一台服务器迁移到另一个时，哪些文件从传输中排除。
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 06/04/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: 8f0f16f14ccf9099af8ff8bb8b27209c75c87cfc
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284471"
---
# <a name="storage-migration-service-frequently-asked-questions-faq"></a>存储迁移服务方面的常见问题 (FAQ)

本主题包含有关使用常见问题 (Faq) 解答[存储迁移服务](overview.md)将服务器迁移。

## <a name="excluded-files"></a> 从传输中排除哪些文件和文件夹？

存储迁移服务不会传输文件或文件夹，我们知道可能会影响 Windows 运行。 具体而言，以下是什么我们不会传输或移动到 PreExistingData 文件夹在目标上：

- Windows，程序文件，程序文件 (x86)，程序数据的用户
- $Recycle.bin recycler、 Recycled、 系统卷信息 $UpgDrv$，$SysReset，~ BT，$Windows.~ LS、 Windows.old、 启动、 恢复、 文档和设置 $Windows。
- pagefile.sys、 hiberfil.sys、 swapfile.sys、 winpepge.sys、 config.sys、 bootsect.bak、 bootmgr、 bootnxt
- 任何文件或文件夹与冲突的源服务器上排除的文件夹在目标上。 <br>例如，如果在源上没有 N:\Windows 文件夹，并获取映射到 C:\卷的目标上，它不会传输 — 而不考虑它所包含的内容 — 因为它会干扰目标上的 C:\Windows 系统文件夹。

## <a name="domain-migration"></a> 域迁移支持？

存储迁移服务不允许 Active Directory 域之间迁移。 服务器之间的迁移将始终将目标服务器加入到同一个域。 在 Active Directory 林中的不同域中，可以使用迁移凭据。 存储迁移服务支持工作组之间迁移。  

## <a name="cluster-support"></a> 为支持作为源或目标的群集？

存储迁移服务目前不会在 Windows Server 2019 群集之间迁移。 我们计划在未来版本的存储迁移服务中添加群集支持。

## <a name="local-principals"></a> 本地组和本地用户迁移？

存储迁移服务目前不会迁移本地用户或 Windows Server 2019 中的本地组。 我们计划添加本地用户和本地组迁移存储迁移服务的未来版本中支持。

## <a name="domain-controller"></a> 域控制器迁移支持？

存储迁移服务目前不会迁移 Windows Server 2019 中的域控制器。 解决方法是，前提是 Active Directory 域中具有多个域控制器，降级域控制器，然后迁移它，然后将提升目标转换完成后。 我们计划在未来版本的存储迁移服务中添加域控制器迁移支持。

## <a name="share-attributes"></a> 存储迁移服务迁移哪些属性？

存储迁移服务迁移所有标志、 设置和安全性的 SMB 共享。 标志，用于存储迁移服务将迁移该列表包括：

    - 共享状态
    - 可用性类型
    - 共享类型
    - 文件夹枚举模式 *（也称为基于访问的枚举或 ABE）*
    - 缓存模式
    - 释放模式
    - Smb 实例
    - CA 超时
    - 并发用户限制
    - 持续可用
    - 描述           
    - 对数据进行加密
    - 标识远程处理
    - 基础结构
    - 名称
    - 路径
    - 作用域
    - 作用域名称
    - 安全描述符
    - 卷影副本
    - 特殊
    - 临时

## <a name="move-db"></a> 我是否可以迁移存储迁移服务数据库？

存储迁移服务使用隐藏的 c:\programdata\microsoft\storagemigrationservice 文件夹中的默认情况下安装的可扩展存储引擎 (ESE) 数据库。 此数据库将随着添加作业，并且传输已完成，并可以在迁移后使用大量的驱动器空间数以百万计的文件，如果不删除作业。 如果需要移动数据库，请执行以下步骤：

1. 停止业务流程协调程序计算机上的"存储迁移服务"服务。
2. 取得所有权的`%programdata%/Microsoft/StorageMigrationService`文件夹
3. 添加你的用户帐户具有完全控制对此进行共享和所有其文件和子文件夹。
4. 将文件夹移动到另一个驱动器中，业务流程协调程序计算机上。
5. 设置以下注册表 REG_SZ 值：

    HKEY_Local_Machine\Software\Microsoft\SMS DatabasePath =*的其他卷上的新数据库文件夹路径*。 
6. 请确保系统具有完全控制权限的所有文件和文件夹的子文件夹
7. 删除你自己的帐户权限。
8. 启动"存储迁移服务"服务。

## <a name="non-windows"></a> 可以迁移从 Windows Server 以外的源？

在 Windows Server 2019 中提供的存储迁移服务版本支持从 Windows Server 2003 和更高版本操作系统迁移。 在 Linux、 Samba、 NetApp、 EMC 或其他 SAN 和 NAS 存储设备，但当前不能迁移。 我们计划为实现这一初始 Linux Samba 支持的存储迁移服务的未来版本中。

## <a name="previous-versions"></a> 可以将迁移以前的文件版本？

在 Windows Server 2019 中提供的存储迁移服务版本不支持迁移以前版本 （使其与卷影复制服务） 的文件。 仅最新版本将迁移。 

## <a name="ntfs-refs"></a> 是否可以迁移从 NTFS 到 REFS？

在 Windows Server 2019 中提供的存储迁移服务版本不支持从 NTFS 迁移到 REFS 文件系统。 对 NTFS 和 REFS 为 ReFS，可以从 NTFS 进行迁移。 这是默认设置，由于功能、 元数据和 ReFS 不会重复从 NTFS 其他方面的许多差异。 ReFS 旨在作为应用程序工作负荷文件系统，不是常规文件系统。 有关详细信息，请参阅[复原文件系统 (ReFS) 概述](../refs/refs-overview.md)

## <a name="consolidate-servers"></a> 可以将多个服务器合并到一台服务器？

在 Windows Server 2019 中提供的存储迁移服务版本不支持将多个服务器整合到一台服务器。 合并的示例将迁移三个单独的源服务器-可能具有相同的共享名，并且到的虚拟化这些路径和共享，以防止任何重叠或冲突时，一台新服务器上的本地文件路径-然后回答所有这三个以前的服务器名称和 IP 地址。 存储迁移服务的未来版本中，我们可以添加此功能。  

## <a name="optimize"></a> 优化的清单和传输性能

存储迁移服务包含多线程的读取和复制引擎，即我们旨在快速既是，以及迁移完美的数据保真度缺少多个文件复制工具中的存储迁移服务代理服务。 虽然默认配置将为最适合于很多客户，有如何提高 SMS 清单和传输过程中的性能。

- **Windows Server 2019 用于目标操作系统。** Windows Server 2019 包含存储迁移服务代理服务。 如果您安装此功能，将迁移到 Windows Server 2019 目标的所有传输采用直接访问的源和目标之间都运行。 此服务上运行业务流程协调程序在传输期间如果目标计算机是 Windows Server 2012 R2 或 Windows Server 2016 中，这意味着传输双跃点，并且将慢得多。 如果有多个作业运行 Windows Server 2012 R2 或 Windows Server 2016 的目标，业务流程协调程序将成为瓶颈。 

- **更改默认传输线程。** 存储迁移服务代理服务在某个给定的作业中同时复制 8 个文件。 可以通过调整以下注册表 REG_DWORD 值名称以十进制运行 SMS 代理每个节点上增加同时复制线程的数：

    HKEY_Local_Machine\Software\Microsoft\SMSProxy   FileTransferThreadCount

   有效范围是 1 到 128 在 Windows Server 2019。 更改后必须重新启动包含在迁移中的所有计算机上的存储迁移服务代理服务。 请谨慎使用此设置;更多内核数、 存储性能和网络带宽，可能需要将其设置为更高版本。 设置过高可能导致与默认设置比较的性能降低。 更高版本的 SMS 计划会试探性地更改基于 CPU、 内存、 网络和存储的线程设置的能力。

- **添加内核和内存。**  我们强烈建议在源、 业务流程协调程序和目标计算机具有至少两个处理器核心或两个 Vcpu、 和的详细信息可显著帮助清单和传输性能，尤其是在结合了 FileTransferThreadCount （上述）。 传输大于常规 Office 格式的文件时 (千兆字节或更高版本) 传输性能将会提高从默认 2 GB 最小值的更多内存。

- **创建多个作业。** 当创建包含多个服务器源的作业，以串行方式的清单，传输，联系每台服务器和直接转换。 这意味着每个服务器必须完成另一台服务器启动之前其阶段。 若要以并行方式运行更多的服务器，只需与每个作业仅包含一个服务器创建多个作业。 SMS 支持最多 100 同时运行的作业，这意味着单个业务流程协调程序可以并行执行多个 Windows Server 2019 目标计算机。 我们不建议运行多个并行作业，如果目标计算机是 Windows Server 2016 或 Windows Server 2012 R2 而无需在目标上运行的 SMS 代理服务，业务流程协调程序必须执行所有将传输本身，并且可能会变得瓶颈。 若要在单个作业中的并行运行的服务器的功能是我们计划在短信的更高版本中添加的功能。

- **RDMA 网络中使用 SMB 3。** 如果从 Windows Server 2012 或更高版本的源计算机、 SMB 3.x 支持 SMB 直通模式和 RDMA 网络传输。 RDMA 传输大多数 CPU 开销从转为主板 Cpu 板载 NIC 处理器，减少延迟和服务器的 CPU 使用率。 此外，RDMA 网络的等 ROCE 和 iWARP 通常具有比典型 TCP/以太网，其中包括 25、 50 和 100 Gb 每个接口的速度要高得多的带宽。 通常使用 SMB 直通将从本身的存储到网络的传输速度限制。   

- **使用 SMB 3 多通道。** 如果从 Windows Server 2012 或更高版本的源计算机的传输，SMB 3.x 支持多渠道的副本可以极大地提高文件的复制性能。 此功能会自动生效，只要源和目标均已：

   - 多个网络适配器
   - 支持接收方缩放 (RSS) 的一个或多个网络适配器
   - 通过使用 NIC 组合配置的多个网络适配器之一
   - 一个或多个支持 RDMA 的网络适配器

- **更新驱动程序。** 根据需要，安装最新的供应商存储和机箱固件和驱动程序、 最新的供应商 HBA 驱动程序、 最新的供应商 BIOS/UEFI 固件、 最新的供应商网络驱动程序，以及最新的母板芯片组驱动程序上源、 目标和业务流程协调程序服务器。 根据需要重启节点。 请查看配置共享存储和网络硬件的硬件供应商文档。

- **启用高性能的处理。** 确保服务器的 BIOS/UEFI 设置启用高性能，例如禁用 C-State、设置 QPI 速度、启用 NUMA 和设置最高内存频率。 确保 Windows Server 中的电源管理设置为高性能。 根据需要重启。 别忘了在完成迁移之后返回到相应的状态。 

- **优化硬件**评审[性能优化指南 Windows Server 2016 的](https://docs.microsoft.com/windows-server/administration/performance-tuning/)进行优化的业务流程协调程序和目标计算机运行 Windows Server 2019 及 Windows Server 2016。 [网络子系统性能调整](https://docs.microsoft.com/windows-server/networking/technologies/network-subsystem/net-sub-performance-tuning-nics)部分包含尤其有价值的信息。

- **使用较快的存储。** 尽管它可能很难进行升级源计算机存储的速度，应确保目标存储已至少为快地写入 IO 性能的源是在读取 IO 性能以确保在传输中不存在任何不必要的瓶颈。 如果目标是 VM，请确保，至少对于迁移而言，它在运行最快的存储层的虚拟机监控程序主机，如闪存层上或存储空间直通 HCI 群集使用镜像的所有闪存或混合空格。 SMS 迁移完成后 VM 可以在实时迁移到速度较慢的层或主机。

- **更新防病毒。** 请始终确保您的源和目标运行防病毒软件，以确保性能开销最小的最新修补的版本。 为测试，你可以*暂时*正在清点或迁移的源和目标服务器上文件夹的扫描中排除。 如果传输性能得到改进，请联系防病毒软件供应商联系，为说明或防病毒软件的更新的版本或预期的性能下降的说明。

## <a name="give-feedback"></a> 我要提供反馈、 报告 bug，或获取支持的选项有哪些？

若要提供有关存储迁移服务的反馈：

- 使用包含在 Windows 10 中，单击"建议功能"，并指定"Windows Server"的类别和子类别的"存储迁移"的反馈中心工具
- 使用[Windows Server UserVoice](https://windowsserver.uservoice.com)站点
- 电子邮件 smsfeed@microsoft.com

文件的 bug:

- 使用包含在 Windows 10 中，单击"报告问题"，并指定"Windows Server"的类别和子类别的"存储迁移"的反馈中心工具
- 通过支持案例[Microsoft 支持部门](https://support.microsoft.com)

若要获取支持：

 - 可以将问题发布[Windows Server 技术社区](https://techcommunity.microsoft.com/t5/Windows-Server/ct-p/Windows-Server)
 - 将它发布在[Windows Server 2019 Technet 论坛](https://social.technet.microsoft.com/Forums/en-US/home?forum=ws2019&filter=alltypes&sort=lastpostdesc) 
 - 通过支持案例[Microsoft 支持部门](https://support.microsoft.com)


## <a name="see-also"></a>请参阅

- [存储迁移服务概述](overview.md)
