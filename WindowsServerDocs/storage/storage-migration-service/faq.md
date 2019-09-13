---
title: 存储迁移服务常见问题（FAQ）
description: 有关存储迁移服务的常见问题，如从一台服务器迁移到另一台服务器时，从传输中排除的文件。
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 08/19/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: a1e195ab755dfd0b61cc4201f43373421ce51aa2
ms.sourcegitcommit: 86350de764b89ebcac2a78ebf32631b7b5ce409a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2019
ms.locfileid: "70923550"
---
# <a name="storage-migration-service-frequently-asked-questions-faq"></a>存储迁移服务常见问题（FAQ）

本主题包含有关使用[存储迁移服务](overview.md)迁移服务器的常见问题解答（faq）的解答。

## <a name="what-files-and-folders-are-excluded-from-transfers"></a>哪些文件和文件夹已从传输中排除？

存储迁移服务不会传输我们知道可能会干扰 Windows 操作的文件或文件夹。 具体而言，我们不会传输或移动到目标的 PreExistingData 文件夹中：

- Windows，Program Files，Program Files （x86），程序数据，用户
- $Recycle bin、Recycler、回收、系统卷信息、$UpgDrv $、$SysReset $Windows. ~ BT，$Windows. ~ LS，Windows .old，启动，恢复，文档和设置
- 页面文件 .sys，hiberfil.sys，交换文件，winpepge，bootsect.exe，，bootmgr，bootnxt。
- 源服务器上与目标上的已排除文件夹冲突的任何文件或文件夹。 <br>例如，如果源中有一个 N:\Windows 文件夹，并将其映射到 C：\目标上的卷不会传输（无论它包含什么内容），因为它会干扰目标上的 C：\Windows 系统文件夹。

## <a name="are-domain-migrations-supported"></a>是否支持域迁移？

存储迁移服务不允许在 Active Directory 域之间进行迁移。 服务器之间的迁移将始终将目标服务器加入到同一个域。 你可以使用 Active Directory 林中不同域的迁移凭据。 存储迁移服务支持工作组间的迁移。  

## <a name="are-clusters-supported-as-sources-or-destinations"></a>群集是否支持作为源或目标？

存储迁移服务目前不在 Windows Server 2019 中的群集之间迁移。 我们计划在未来版本的存储迁移服务中添加群集支持。

## <a name="do-local-groups-and-local-users-migrate"></a>本地组和本地用户是否迁移？

存储迁移服务当前不迁移 Windows Server 2019 中的本地用户或本地组。 我们计划在未来版本的存储迁移服务中添加本地用户和本地组迁移支持。

## <a name="is-domain-controller-migration-supported"></a>是否支持域控制器迁移？

存储迁移服务当前不迁移 Windows Server 2019 中的域控制器。 作为一种解决方法，只要 Active Directory 域中有多个域控制器，则在迁移域控制器之前将其降级，然后在剪切完成后将目标提升。 我们计划在未来版本的存储迁移服务中添加域控制器迁移支持。

## <a name="what-attributes-are-migrated-by-the-storage-migration-service"></a>存储迁移服务迁移哪些属性？

存储迁移服务迁移 SMB 共享的所有标志、设置和安全性。 存储迁移服务迁移的标志列表包括：

    - 共享状态
    - 可用性类型
    - 共享类型
    - 文件夹枚举模式 *（又称基于访问权限的枚举或 ABE）*
    - 缓存模式
    - 租赁模式
    - Smb 实例
    - CA 超时
    - 并发用户限制
    - 持续可用
    - 描述           
    - 加密数据
    - 标识远程处理
    - 基础结构
    - 名称
    - Path
    - 划分
    - 作用域名称
    - 安全描述符
    - 卷影副本
    - 专题
    - 权宜之计

## <a name="can-i-consolidate-multiple-servers-into-one-server"></a>是否可以将多个服务器合并到一个服务器？

Windows Server 2019 中随附的存储迁移服务版本不支持将多个服务器合并到一台服务器中。 合并的一个示例是将三个单独的源服务器（可能具有相同的共享名称和本地文件路径）迁移到单个新服务器上，该服务器虚拟化这些路径和共享，以防止任何重叠或冲突，然后回答所有三个以前的服务器名称和 IP 地址。 我们可以在未来版本的存储迁移服务中添加此功能。 

## <a name="can-i-migrate-from-sources-other-than-windows-server"></a>能否从 Windows Server 之外的源进行迁移？

Windows Server 2019 中随附的存储迁移服务版本支持从 Windows Server 2003 和更高版本的操作系统进行迁移。 你还可以从使用 Samba 的 Linux 服务器或设备迁移存储;为此，请在运行 Windows Server 1903 或更高版本的服务器上运行存储迁移服务。

## <a name="can-i-migrate-previous-file-versions"></a>能否迁移以前的文件版本？

Windows Server 2019 中随附的存储迁移服务版本不支持迁移文件的以前版本（与卷影复制服务一起创建）。 仅迁移当前版本。 

## <a name="optimizing-inventory-and-transfer-performance"></a>优化库存和传输性能

存储迁移服务包含一个名为 "存储迁移服务代理" 服务的多线程读取和复制引擎，该服务旨在实现快速，并在许多文件复制工具中引入完美的数据保真度。 虽然对于许多客户而言，默认配置是最佳的，但在清单和传输过程中可以通过多种方式来提高 SMS 性能。

- **为目标操作系统使用 Windows Server 2019。** Windows Server 2019 包含存储迁移服务代理服务。 当你安装此功能并迁移到 Windows Server 2019 目标时，所有传输操作都将作为源和目标之间的直接可见。 如果目标计算机是 Windows Server 2012 R2 或 Windows Server 2016，则在传输过程中，此服务在 orchestrator 运行，这意味着传输双跃点，将会慢得多。 如果有多个作业使用 Windows Server 2012 R2 或 Windows Server 2016 目标运行，则协调器会成为瓶颈。 

- **更改默认传输线程。** 存储迁移服务代理服务在给定的作业中同时复制8个文件。 你可以通过在运行存储迁移服务代理的每个节点上，通过在 decimal 中调整以下注册表 REG_DWORD 值名称，增加同时复制线程数：

    HKEY_Local_Machine\Software\Microsoft\SMSProxy
    
    FileTransferThreadCount

   Windows Server 2019 中的有效范围为1到128。 更改之后，必须在所有参与迁移的计算机上重新启动存储迁移服务代理服务。 使用此设置时要小心;将其设置得更高可能需要额外的核心、存储性能和网络带宽。 如果将它设置得太高，则可能会导致性能下降，而不是默认设置。

- **添加内核和内存。**  强烈建议源、orchestrator 和目标计算机至少有两个处理器核心或两个个 vcpu，并且更多可能会显著地有助于清点和传输性能，尤其是在与 FileTransferThreadCount （以上）结合使用时。 当传输的文件大于常用的 Office 格式（千兆字节或更大）时，传输性能将从超过默认2GB 最小值的更多内存中受益。

- **创建多个作业。** 创建具有多个服务器源的作业时，将按串行方式联系每个服务器以进行库存、传输和切换。 这意味着每个服务器必须在另一台服务器启动之前完成其阶段。 若要并行运行多个服务器，只需创建多个作业，每个作业只包含一个服务器。 SMS 最多支持100个同时运行的作业，这意味着单个 orchestrator 可以并行化许多 Windows Server 2019 目标计算机。 如果目标计算机是 Windows Server 2016 或 Windows Server 2012 R2，而不是在目标计算机上运行 SMS 代理服务，则我们不建议运行多个并行作业，orchestrator 必须执行所有传输本身，并可能成为产生. 服务器在单个作业中并行运行的功能是我们计划在更高版本的 SMS 中添加的一项功能。

- **将 SMB 3 用于 RDMA 网络。** 如果是从 Windows Server 2012 或更高版本的源计算机传输，则 SMB 2.x 支持 SMB Direct 模式和 RDMA 网络。 RDMA 将从主板 Cpu 传输的大多数 CPU 成本转移到内置 NIC 处理器，从而减少延迟和服务器 CPU 利用率。 此外，RDMA 网络（如 ROCE 和 iWARP）通常比典型的 TCP/以太网具有更高的带宽，其中包括25、50和每个接口的100Gb 速度。 使用 SMB 直通通常会将传输速度限制从网络移到存储自身。   

- **使用 SMB 3 多通道。** 如果是从 Windows Server 2012 或更高版本的源计算机进行传输，则 SMB 2.x 支持多通道副本，这些副本可以极大地提高文件复制性能。 只要源和目标都具有，此功能便会自动运行：

   - 多个网络适配器
   - 支持接收方缩放（RSS）的一个或多个网络适配器
   - 使用 NIC 组合配置的更多网络适配器之一
   - 一个或多个支持 RDMA 的网络适配器

- **更新驱动程序。** 根据需要，安装最新的供应商存储和机箱固件及驱动程序、最新的供应商 HBA 驱动程序、最新的供应商 BIOS/UEFI 固件、最新的供应商网络驱动程序和最新的主板芯片驱动程序。服务器. 根据需要重启节点。 请查看配置共享存储和网络硬件的硬件供应商文档。

- **启用高性能处理。** 确保服务器的 BIOS/UEFI 设置启用高性能，例如禁用 C-State、设置 QPI 速度、启用 NUMA 和设置最高内存频率。 确保 Windows Server 中的电源管理设置为高性能。 根据需要重启。 完成迁移后，请不要忘记将这些状态返回到适当的状态。 

- **调整硬件**查看[Windows server 2016 的性能调整准则](https://docs.microsoft.com/windows-server/administration/performance-tuning/)，以优化运行 windows server 2019 和 windows server 2016 的 orchestrator 和目标计算机。 [网络子系统性能优化](https://docs.microsoft.com/windows-server/networking/technologies/network-subsystem/net-sub-performance-tuning-nics)部分包含特别有用的信息。

- **使用更快的存储。** 尽管很难升级源计算机存储速度，但在源处于读取 IO 性能时，应确保目标存储至少具有更快的速度，因为这样可以确保传输中没有不必要的瓶颈。 如果目标是 VM，请确保至少出于迁移目的，它在虚拟机监控程序主机的最快存储层中运行，例如在闪存层上，或使用镜像的全部闪存或混合空间存储空间直通 HCI 群集。 SMS 迁移完成后，可以将 VM 实时迁移到慢速层或主机上。

- **更新防病毒。** 请始终确保源和目标正在运行防病毒软件的修补程序的最新版本，以确保最小的性能开销。 作为测试，你可以*暂时*排除在源服务器和目标服务器上进行清点或迁移的文件夹的扫描。 如果提高了传输性能，请与防病毒软件供应商联系，以获取防病毒软件的说明或更新版本，或对预期性能下降的解释。

## <a name="can-i-migrate-from-ntfs-to-refs"></a>能否从 NTFS 迁移到 REFS？

Windows Server 2019 中随附的存储迁移服务版本不支持从 NTFS 迁移到 REFS 文件系统。 你可以从 NTFS 迁移到 NTFS，并将 REFS 迁移到 ReFS。 这是设计使然，因为功能、元数据和其他引用不会从 NTFS 复制的其他方面存在差异。 ReFS 旨在用作应用程序工作负荷文件系统，而不是常规文件系统。 有关详细信息，请参阅[复原文件系统（ReFS）概述](../refs/refs-overview.md) 

## <a name="can-i-move-the-storage-migration-service-database"></a>能否移动存储迁移服务数据库？

存储迁移服务使用默认情况下在隐藏的 c:\programdata\microsoft\storagemigrationservice 文件夹中安装的可扩展存储引擎（ESE）数据库。 此数据库将在添加作业和传输完成时增长，并在迁移数百万个文件后，如果不删除作业，则会占用大量的驱动器空间。 如果数据库需要移动，请执行以下步骤：

1. 停止 orchestrator 计算机上的 "存储迁移服务" 服务。
2. 取得`%programdata%/Microsoft/StorageMigrationService`文件夹的所有权
3. 添加你的用户帐户，以便对该共享及其所有文件和子文件夹拥有完全控制权。
4. 将文件夹移动到 orchestrator 计算机上的另一个驱动器。
5. 设置以下注册表 REG_SZ 值：

    HKEY_Local_Machine\Software\Microsoft\SMS DatabasePath =*指向不同卷上的新数据库文件夹的路径*。 
6. 确保系统对该文件夹的所有文件和子文件夹具有 "完全控制"
7. 删除自己的帐户权限。
8. 启动 "存储迁移服务" 服务。

## <a name="give-feedback"></a>什么是提供反馈、文件 bug 或获取支持的选项？

提供有关存储迁移服务的反馈：

- 使用 Windows 10 中包含的反馈中心工具，单击 "建议功能"，然后指定 "Windows Server" 和 "存储迁移" 子类别的类别。
- 使用[Windows Server UserVoice](https://windowsserver.uservoice.com)站点
- 电子邮件smsfeed@microsoft.com

文件错误：

- 使用 Windows 10 中包含的反馈中心工具，单击 "报告问题"，并指定 "Windows Server" 和 "存储迁移" 子类别的类别
- 通过[Microsoft 支持部门](https://support.microsoft.com)打开支持案例

获得支持：

 - 在[Windows Server 技术社区](https://techcommunity.microsoft.com/t5/Windows-Server/ct-p/Windows-Server)上发布问题
 - [Windows Server 2019 Technet 论坛](https://social.technet.microsoft.com/Forums/en-US/home?forum=ws2019&filter=alltypes&sort=lastpostdesc)上的文章 
 - 通过[Microsoft 支持部门](https://support.microsoft.com)打开支持案例

## <a name="see-also"></a>请参阅

- [存储迁移服务概述](overview.md)
