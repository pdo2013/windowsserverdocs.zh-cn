---
Title: 使用 HYPER-V 的虚拟化域控制器
description: 若要使虚拟化 Windows Server Active Directory 域控制器的 HYPER-V 中时的注意事项
author: MicrosoftGuyJFlo
ms.author: joflore
ms.date: 04/19/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.openlocfilehash: 8a1775a40761e4a489cc39535514d75174edffa5
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442996"
---
# <a name="virtualizing-domain-controllers-using-hyper-v"></a>使用 HYPER-V 的虚拟化域控制器

> 适用于：Windows Server 2016

本主题将更新以使指南适用于 Windows Server 2016。 Windows Server 2012 引入了很多改进虚拟化的域控制器 (Dc)，包括安全措施以防止在虚拟 Dc 的 USN 回滚和克隆虚拟域控制器的功能。 有关这些改进的详细信息，请参阅[Active Directory 域服务 (AD DS) 虚拟化 (级别 100) 简介](../../introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100.md)。

HYPER-V 将合并到单一物理计算机上的不同服务器角色。 本指南介绍为 32 位或 64 位来宾操作系统运行的域控制器。

## <a name="planning-to-virtualize-domain-controllers"></a>规划虚拟化域控制器

本部分介绍了 hyper-v 服务器的硬件要求如何避免单点故障，选择适当类型的配置的 HYPER-V 服务器和虚拟机和安全性和性能的决策。

## <a name="hyper-v-requirements"></a>HYPER-V 要求

若要安装和使用 HYPER-V 角色，您必须：

   - **X64 处理器**
      - HYPER-V 是基于 x64 的版本的 Windows Server 2008 或更高版本中可用。  
   - **硬件辅助虚拟化**
      - 此功能现已推出虚拟化选项，具体而言，Intel 虚拟化技术 (Intel VT) 或 AMD 虚拟化 (AMD-V) 的处理器。  
   - **硬件数据执行保护 (DEP)**
      - 硬件 DEP 必须可用且已启用。 具体而言，必须启用 Intel XD 位 （执行禁用位） 或 AMD NX 位 （无执行位）。  

## <a name="avoid-creating-single-points-of-failure"></a>避免创建单一故障点

应尝试避免在计划部署在虚拟域控制器时创建潜在单一故障点。 您可以避免通过实现系统冗余引入潜在单一故障点。 例如，记住增加了管理成本的可能性时考虑以下建议：

1. 每个域运行至少两个虚拟化的域控制器，不同的虚拟化在主机上，从而减少了丢失的所有域控制器，如果单个虚拟化主机发生故障的风险。  
2. 根据建议的其他技术，多元化 （使用不同的 Cpu、 主板、 网络适配器或其他硬件） 的硬件上的域控制器正在运行。 硬件因卡而异限制可能由特定于供应商配置、 驱动程序，或一段或类型的硬件故障导致的损害。  
3. 如果可能，应位于不同地区的世界中的硬件上运行的域控制器。 这有助于减少灾难或影响域控制器位于的站点的故障的影响。  
4. 维护每个域中的物理域控制器。 这应会影响使用该平台的所有主机系统虚拟化平台不正常工作的风险。  

## <a name="security-considerations"></a>安全注意事项

即使该计算机是仅在已加入域或工作组计算机，必须为可写域控制器，请仔细管理在其运行虚拟域控制器的主机计算机。 这是一个重要的安全的考虑因素。 不善的主机容易受到提升权限攻击，恶意用户获得访问权限时，会发生和未授权或以合法方式分配的系统权限。 恶意用户可以使用此类型的攻击破坏所有虚拟机，域和林中的此计算机承载。

请务必规划虚拟化域控制器时记住以下安全注意事项：

   - 承载虚拟的、 可写域控制器应考虑的所有域和林的那些域控制器属于默认域管理员凭据与计算机的本地管理员。  
   - 若要避免安全和性能问题的建议的配置是运行服务器核心安装的 Windows Server 2008 或更高版本，与 HYPER-V 以外的任何应用程序的主机。 此配置限制应用程序和服务器，这应该产生更高的性能和减少应用程序和服务可能被恶意利用的攻击的计算机或网络安装的服务的数。 此类型的配置的效果被称为减少了的攻击面。 在分支机构或不能令人满意地保护其他位置中，建议只读域控制器 (RODC)。 如果存在单独的管理网络，我们建议，仅向管理网络连接主机。  
   - 你的域控制器上使用 Bitlocker，自 Windows Server 2016 可以使用还授予来宾密钥材料的虚拟 TPM 功能来解锁系统卷。
   - [受保护的构造和受防护的 Vm](/it-server/WindowsServerDocs/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md)可以提供其他控制来保护你的域控制器。

有关 Rodc 的信息，请参阅[只读域控制器计划和部署指南](../../deploy/rodc/read-only-domain-controller-updates.md)。

有关保护域控制器的详细信息，请参阅[Active Directory 安全安装最佳做法指南](../../plan/security-best-practices/best-practices-for-securing-active-directory.md)。

## <a name="security-boundaries-for-different-host-and-guest-configurations"></a>对于不同的主机和来宾配置的安全边界

使用虚拟机，使可能有多个不同配置的域控制器。 仔细考虑必须授予给虚拟机影响边界和 Active Directory 拓扑中的信任关系的方法。 可能的 Active Directory 域控制器和主机 （HYPER-V 服务器） 和其来宾计算机 （虚拟机的 HYPER-V 服务器上运行） 配置以下表所述。

|Machine|配置 1|Configuration 2|
|-------|---------------|---------------|
|主机|工作组或成员的计算机|工作组或成员的计算机|
|Guest|域控制器|工作组或成员的计算机|

![](media/virtualized-domain-controller-architecture/Dd363553.f44706fd-317e-4f0b-9578-4243f4db225f(WS.10).gif)

   - 在主计算机上的管理员可写域控制器来宾上具有相同的访问权限以域管理员，并必须将这种情况下处理。 对于 RODC guest 主机计算机的管理员具有相同的访问权限以本地管理员身份访客 RODC 上。   
   - 如果主机已加入到同一个域，虚拟机中的域控制器在主机上具有管理权限。 没有恶意用户破坏所有虚拟机，如果恶意用户首次获得访问权限为虚拟机 1 的机会。 这被称为的攻击向量。 如果有多个域或林的域控制器，这些域应具有在所有域，为受信任的一个域管理员的集中式的管理。  
   - 从虚拟机 1 攻击的可能性存在即使虚拟机 1 作为 RODC 安装。 尽管 RODC 的管理员显式不具有域管理员权限，但 RODC 可以用于将策略发送到主计算机。 这些策略可能包括启动脚本。 如果此操作成功，主机计算机遭到破坏，且不然后能用来破坏主机计算机上的其他虚拟机。  

## <a name="security-of-vhd-files"></a>VHD 文件的安全性

虚拟域控制器的 VHD 文件相当于物理域控制器的物理硬盘驱动器。 在这种情况下，它应使用相同数量的护理进入保护物理域控制器的硬盘驱动器进行保护。 请确保只有可靠且受信任的管理员允许对域控制器的 VHD 文件的访问。

## <a name="rodcs"></a>RODCs

Rodc 的一个好处是能够将它们放在物理安全性不能保证的位置，例如在分支机构。 可以使用 Windows BitLocker 驱动器加密来保护自己的 VHD 文件 (不是文件系统其中) 不会受到攻击的被盗的物理磁盘的主机上。 
<!-- Removed link to Windows Server 2008 Hyper-V and BitLocker Drive Encryption (http://go.microsoft.com/fwlink/?linkid=123534). Link is dead. -->

## <a name="performance"></a>性能

新的微内核 64 位体系结构，有大量增加的 HYPER-V 性能上与先前的虚拟化平台中。 为了获得最佳的主机性能主机应该是服务器核心安装的 Windows Server 2008 或更高版本，并且它不应安装 HYPER-V 以外的服务器角色。

虚拟机的性能取决于特定于工作负荷。 若要保证 Active Directory 性能令人满意，测试特定的拓扑。 评估当前的工作负荷一段时间的可靠性和性能监视器 (Perfmon.msc) 之类的工具或[Microsoft 评估与规划 (MAP) 工具包](https://go.microsoft.com/fwlink/?linkid=137077)。 映射工具可能也很有用，如果你想要清点的所有服务器和网络中当前存在的服务器角色。

若要大致了解虚拟化的域控制器的性能，以下的性能测试已执行的[Active Directory 性能测试工具 (ADTest.exe)](https://go.microsoft.com/fwlink/?linkid=137088)。

轻型目录访问协议 (LDAP) 测试物理域控制器都相同的服务器上具有 ADTest.exe 的物理域控制器，然后托管的虚拟机上运行。 只有一个逻辑处理器用于物理计算机，并只有一个虚拟处理器用于虚拟机轻松地与 100%的 CPU 利用率。 下表中的字母和数字在每个测试后的括号指示 ADTest.exe 中的特定测试。 此数据显示，虚拟化的域控制器性能是 88 为 98%的物理域控制器性能。

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>度量值</th>
<th>测试</th>
<th>物理</th>
<th>虚拟</th>
<th>增量</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>搜索数/秒</p></td>
<td><p>公用名 (L1) 的基本范围中搜索</p></td>
<td><p>11508</p></td>
<td><p>10276</p></td>
<td><p>-10.71%</p></td>
</tr>
<tr class="even">
<td><p>搜索数/秒</p></td>
<td><p>搜索 (L2) 的基本范围中的属性集</p></td>
<td><p>10123</p></td>
<td><p>9005</p></td>
<td><p>-11.04%</p></td>
</tr>
<tr class="odd">
<td><p>搜索数/秒</p></td>
<td><p>搜索 (L3) 的基本范围中的所有属性</p></td>
<td><p>1284</p></td>
<td><p>1242</p></td>
<td><p>-3.27%</p></td>
</tr>
<tr class="even">
<td><p>搜索数/秒</p></td>
<td><p>搜索子树范围 (L6) 中的常见名称</p></td>
<td><p>8613</p></td>
<td><p>7904</p></td>
<td><p>-8.23%</p></td>
</tr>
<tr class="odd">
<td><p>成功绑定数/秒</p></td>
<td><p>执行快速的绑定 (B1)</p></td>
<td><p>1438</p></td>
<td><p>1374</p></td>
<td><p>-4.45%</p></td>
</tr>
<tr class="even">
<td><p>成功绑定数/秒</p></td>
<td><p>执行简单绑定 (B2)</p></td>
<td><p>611</p></td>
<td><p>550</p></td>
<td><p>-9.98%</p></td>
</tr>
<tr class="odd">
<td><p>成功绑定数/秒</p></td>
<td><p>使用 NTLM 执行绑定 (B5)</p></td>
<td><p>1068</p></td>
<td><p>1056</p></td>
<td><p>-1.12%</p></td>
</tr>
<tr class="even">
<td><p>Writes/sec</p></td>
<td><p>编写多个属性 (W2)</p></td>
<td><p>6467</p></td>
<td><p>5885</p></td>
<td><p>-9.00%</p></td>
</tr>
</tbody>
</table>

若要确保性能令人满意，集成组件 (IC) 已安装以允许来宾操作系统为使用"启发方法"或识别虚拟机监控程序的综合的驱动程序。 在安装过程中，它可能需要使用模拟集成驱动电子设备 (IDE) 或网络适配器驱动程序。 在生产环境中，您应替换为这些仿真驱动程序合成驱动程序，以提高性能。

当监视的虚拟机的性能与可靠性和性能管理器 (Perfmon.msc) 时，虚拟机中的 CPU 信息不会由于虚拟 CPU 计划物理处理器的方法完全准确。 当你想要获取的 HYPER-V 服务器运行的虚拟机的 CPU 信息时，请在主分区中使用的 HYPER-V 虚拟机监控程序逻辑处理器计数器。

有关性能优化的 AD DS 和 HYPER-V，请参阅[性能优化指南 Windows Server 2016 的](../../../../administration/performance-tuning/index.md)。
<!-- Updated to 2016 perf guidance -->

此外，不打算使用差异磁盘 VHD，因为差异磁盘 VHD 可能会降低性能配置为域控制器的虚拟机上。 若要详细了解 HYPER-V 磁盘类型，包括差异磁盘，请参阅[新的虚拟硬盘向导](http://go.microsoft.com/fwlink/?linkid=137279)。
<!-- Couldn't find an equivalent WS 2016 Hyper-V article. -->

托管的虚拟环境中的 AD DS 有关的其他信息，请参阅[Active Directory 域控制器托管的虚拟环境中的主机时应考虑的事项](https://go.microsoft.com/fwlink/?linkid=141292)Microsoft 知识库中。

## <a name="deployment-considerations-for-virtualized-domain-controllers"></a>有关虚拟化的域控制器的部署注意事项

有几个常见的虚拟机做法，应避免在将域控制器和时间同步和存储的特殊注意事项时。

## <a name="virtualization-deployment-practices-to-avoid"></a>若要避免的虚拟化部署实践

虚拟化平台，例如 HYPER-V，提供许多便利功能，使管理、 维护、 备份和迁移计算机更容易。 但是，下列常见部署方案和功能不应应用于虚拟域控制器：

- 若要确保持续性的 Active Directory 写入，不要部署虚拟域控制器的数据库文件 (Active Directory 数据库 (NTDS。DIT)，日志和 SYSVOL) 上虚拟 IDE 磁盘。 相反，创建第二个 VHD 附加到虚拟 SCSI 控制器，确保数据库、 日志和 SYSVOL 放在虚拟机的 SCSI 磁盘上安装域控制器期间。  
- 未配置为域控制器的虚拟机上实现差异磁盘虚拟硬盘 (Vhd)。 这使得很容易恢复到以前的版本，并且还会降低性能。 有关 VHD 类型的详细信息，请参阅[新的虚拟硬盘向导](https://go.microsoft.com/fwlink/?linkid=137279)。  
- 不要部署新的 Active Directory 域和林上的 Windows Server 操作系统，而不首先准备使用系统准备工具 (Sysprep) 副本。 有关运行 Sysprep 的详细信息，请参阅[Sysprep （系统准备） 概述](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview)

   > [!WARNING]
   > 不支持在域控制器上运行 Sysprep。

- 为了帮助避免出现潜在的更新序列号 (USN) 回滚情况，不要使用表示一个已部署的域控制器来部署其他域控制器的 VHD 文件的副本。 有关 USN 回滚的详细信息，请参阅 [USN 和 USN 回滚](#usn-and-usn-rollback)。
   - Windows Server 2012 及更高版本允许管理员如果他们想要部署其他域控制器时正确准备克隆域控制器映像
- 不使用 HYPER-V 导出功能导出虚拟机正在运行的域控制器。
  - 与 Windows Server 2012 和更高版本导出和导入的域控制器虚拟来宾处理就像处理非权威还原检测到生成 ID 的更改并不将其配置为克隆。
  - 确保你未使用来宾再导出。
    - 可以使用 HYPER-V 复制来保留域控制器的第二个非活动副本。 如果启动复制的映像时，还需要为不导出 DC 来宾映像后使用源同样执行适当的清理。

## <a name="physical-to-virtual-migration"></a>物理到虚拟迁移

System Center Virtual Machine Manager (VMM) 2008年提供了统一的管理物理机和虚拟机。 它还提供将物理机迁移到虚拟机的功能。 此过程被称为物理到虚拟机转换 （P2V 转换）。 在 P2V 转换过程中，新的虚拟机和物理域控制器要迁移必须未在运行在同一时间，以避免的 USN 回滚的情况下，如中所述[USN 和 USN 回滚](#usn-and-usn-rollback)。

您应该执行 P2V 转换使用离线模式，以便在重新打开域控制器时，目录数据相一致。 脱机模式选项提供，建议在转换物理服务器向导中。 有关联机模式和脱机模式之间的差异的说明，请参阅[P2V:在 VMM 中将物理计算机转换为虚拟机](https://go.microsoft.com/fwlink/?linkid=155072)。 在 P2V 转换过程在虚拟机应不会连接到网络。 P2V 转换过程完成并验证之后，才应启用虚拟机的网络适配器。 此时，在物理源计算机将关闭。 不会得到重新接至网络的物理源计算机再次重新格式化硬盘之前。

> [!NOTE]
> 有更安全的选项可用于创建新的虚拟域控制器都不会运行创建 USN 回滚的风险。 如果已经有至少一个虚拟 DC，可能通过正则促销优惠，从安装媒体 (IfM)，并还使用域控制器克隆，提升中设置新的虚拟 DC。
这也有助于避免出现硬件问题或可能会遇到与平台相关的问题 P2V 转换的虚拟来宾。

> [!WARNING]
> 若要防止出现与 Active Directory 复制问题，请确保只有一个实例 （物理或虚拟） 的给定域控制器上任何位置的给定网络中存在时间。
> 您可以降低旧克隆难题的可能性：
> 
> - 当运行新的虚拟 DC 时，更改两次使用计算机帐户密码： netdom resetpwd /Server: < 域控制器 >...
> - 导出和导入新的虚拟来宾以强制其成为新的生成 ID，因此数据库调用的 id。
> 

## <a name="using-p2v-migration-to-create-test-environments"></a>使用 P2V 迁移创建测试环境

通过 VMM 的 P2V 迁移可用于创建测试环境。 可以将生产域控制器从物理机迁移到虚拟机不永久关闭生产域控制器的情况下创建测试环境中。 但是，在测试环境必须是生产环境的不同网络上的同一个域控制器的两个实例是否存在。 在 P2V 迁移的测试环境的创建过程中必须格外谨慎，以避免可能会影响您的测试和生产环境的 USN 回退。 下面是可用于使用 P2V 创建测试环境的方法。

一个在生产中域控制器从每个域迁移到使用 P2V 物理到虚拟迁移部分中所述指南 》 中提及的测试虚拟机。 物理生产计算机和测试虚拟机必须位于不同的网络时它们会重新联机。 若要避免在测试环境中的 USN 回滚，是要从物理机迁移到虚拟机的所有域控制器必须都使其脱机。 （您可以执行此操作通过停止 ntds 服务或重新启动计算机在目录服务还原模式 (DSRM)。）域控制器处于脱机状态后，应该向环境引入了没有新的更新。 在 P2V 迁移; 期间，计算机必须保持脱机状态无计算机应恢复联机状态之前的所有计算机都完全都迁移。 若要了解有关 USN 回滚的详细信息，请参阅 USN 和 USN 回滚。

后续的测试域控制器应升级为在测试环境中的副本。

## <a name="time-service"></a>时间服务

对于配置为域控制器的虚拟机，建议你禁用主机系统和来宾操作系统用作域控制器之间的时间同步。 这使来宾域控制器同步时间从域层次结构。

若要禁用 HYPER-V 时间同步提供程序，请关闭 VM，并清除下 Integration Services 的时间同步复选框。

> [!NOTE]
> 本指南具有最近已更新以反映当前的建议同步时间为来宾域控制器从仅域层次结构，而不是之前的建议部分禁用主机之间的时间同步系统和来宾域控制器。

## <a name="storage"></a>存储

若要优化的域控制器虚拟机的性能，并确保持续性的 Active Directory 写入，用于存储操作系统、 Active Directory 和 VHD 文件使用以下建议：

- **来宾存储**。 存储 Active Directory 数据库文件 (Ntds.dit)、 日志文件和 SYSVOL 文件从操作系统文件的单独虚拟磁盘上。 创建连接到虚拟 SCSI 控制器的第二个 VHD 并将数据库、 日志和 SYSVOL 存储在虚拟机的虚拟 SCSI 磁盘上。 虚拟 SCSI 磁盘提供更高的性能相比虚拟 IDE 和它们支持强制单元访问 (FUA)。 FUA 可确保操作系统写入和读取数据直接从介质而跳过所有的缓存机制。

  > [!NOTE]
  > 如果想要将 Bitlocker 用于虚拟 DC 来宾的您需要确保其他卷配置为"自动解除锁定"。
  > 详细了解配置自动解锁中可以找到[启用 BitLockerAutoUnlock](https://docs.microsoft.com/powershell/module/bitlocker/enable-bitlockerautounlock)

- **托管的 VHD 文件的存储**。 建议：托管 VHD 文件存储的建议地址的存储。 获得最佳性能，不要存储在由其他服务或应用程序，例如在其安装主机 Windows 操作系统的系统磁盘经常使用的磁盘上的 VHD 文件。 从主机操作系统在单独分区上每个 VHD 文件和任何其他 VHD 文件存储中。 理想的配置是将存储在单独的物理驱动器上的每个 VHD 文件。  

  主机物理磁盘系统也必须满足**至少一个**以下任意条件，以满足虚拟化工作负载数据完整性的要求：  

   - 系统将使用服务器级磁盘 （SCSI、 光纤通道）。  
   - 系统可确保的磁盘已连接到的电池供电的缓存主机总线适配器 (HBA)。  
   - 系统为存储设备使用的存储控制器 （例如，RAID 系统）。  
   - 系统可确保不间断电源 (UPS) 受磁盘的电源。  
   - 系统可确保磁盘的写入缓存功能被禁用。  

- **传递磁盘与固定 VHD**。 有多种方法来配置虚拟机的存储。 如果使用 VHD 文件，固定大小的 Vhd 将动态 Vhd 比效率更高，因为将在创建时分配固定大小的 Vhd 的内存。 传递磁盘，哪些虚拟机可用于访问物理存储介质，甚至更适用于性能。 传递磁盘实质上是物理磁盘或附加到虚拟机的逻辑单元号 (Lun)。 传递磁盘不支持快照功能。 因此，传递磁盘是首选的硬盘配置，因为不建议使用与域控制器的快照。  

若要降低 Active Directory 数据损坏的可能性，使用虚拟 SCSI 控制器：

   - 托管虚拟域控制器的 HYPER-V 服务器上使用 SCSI 物理驱动器 （而不是 IDE ATA/驱动器）。 如果你不能使用 SCSI 驱动器，请确保在托管虚拟域控制器的 IDE ATA/驱动器上禁用写入缓存。 有关详细信息，请参阅[事件 ID 1539 – 数据库完整性](https://go.microsoft.com/fwlink/?linkid=162419)。
   - 若要保证 Active Directory 的持续性写入，Active Directory 数据库、 日志，并且必须将 SYSVOL 放置的虚拟 SCSI 磁盘上。 虚拟 SCSI 磁盘支持强制单元访问 (FUA)。 FUA 可确保操作系统写入和读取数据直接从介质而跳过所有的缓存机制。  

## <a name="operational-considerations-for-virtualized-domain-controllers"></a>虚拟化的域控制器的操作注意事项

虚拟机运行的域控制器具有不适用于物理计算机运行的域控制器的操作限制。 当你使用虚拟化的域控制器时，有一些虚拟化软件功能和不应使用的实践：

   - 执行不暂停、 停止或林的时间超过逻辑删除生存期的时间段存储在虚拟机中的域控制器的已保存的状态，然后从暂停或保存状态恢复。 执行此操作可能会影响复制。 若要了解如何以确定该林的 tombstone 生存时间，请参阅[确定林的 tombstone 生存时间](https://go.microsoft.com/fwlink/?linkid=137177)。  
   - 请勿复制或克隆虚拟硬盘 (Vhd)。 即使使用位置，以便在来宾 VM 中采取防护措施，单个 Vhd 仍可以复制并导致 USN 回滚。
   - 不需要或使用虚拟域控制器的快照。 从技术上讲支持 Windows Server 2012 和更高版本，它不能替代良好的备份策略。 有几个原因创建 DC 快照或还原快照。
   - 不要配置为域控制器的虚拟机上使用差异磁盘 VHD。 这使得恢复到以前的版本太简单了，并且还会降低性能。  
   - 不要使用正在运行的域控制器虚拟机上的导出功能。  
   - 不要还原域控制器或尝试回滚的任何方式而不是使用受支持的备份的 Active Directory 数据库的内容。 有关详细信息，请参阅[备份和还原的虚拟化域控制器的考虑事项](#backup-and-restore-practices-to-avoid)。  

所有这些建议进行，以帮助避免出现更新序列号 (USN) 回滚。 有关 USN 回滚的详细信息，请参阅 USN 和 USN 回滚。

## <a name="backup-and-restore-considerations-for-virtualized-domain-controllers"></a>备份和还原虚拟化的域控制器的注意事项

备份域控制器是一项关键要求针对任何环境。 备份可在域控制器出现故障或管理错误防范数据丢失。 如果发生这种情况下，有必要回滚域控制器的系统状态，到点的时间之前的失败或错误。 还原到正常状态的域控制器的受支持的方法是域的使用备份 Active Directory 兼容的应用程序中使用，如 Windows Server Backup，若要从当前安装中还原产生的系统状态备份控制器。 有关与 Active Directory 域服务 (AD DS) 使用 Windows Server Backup 的详细信息，请参阅[AD DS 备份和恢复循序渐进指南](https://go.microsoft.com/fwlink/?LinkId=138501)。

使用虚拟机技术 Active Directory 的某些要求还原操作上添加了意义的时间。 例如，如果使用虚拟硬盘 (VHD) 文件的副本还原域控制器，则绕过更新的域控制器的数据库版本后已还原的关键步骤。 复制将继续执行不适当的跟踪号码，从而导致在域控制器副本之间不一致的数据库。 在大多数情况下，此问题出现未检测到复制系统并不报告任何错误，尽管域控制器之间的不一致。

还有一种受支持的方法来执行备份和还原的虚拟化的域控制器：

1. 在来宾操作系统中运行 Windows Server Backup。  

使用 Windows Server 2012 和更高版本的 HYPER-V 主机和来宾，可能需要使用快照、 来宾 VM 导出和导入和 HYPER-V 复制的域控制器的受支持的备份。 所有这些但是不是很适合使用来宾 VM 导出一个小例外创建适当的备份历史记录。

使用 Windows Server 2016 HYPER-V 支持"生产快照"的 HYPER-V 服务器触发的来宾操作系统基于 VSS 的备份和来宾完成的快照时，主机提取 Vhd 并将它们存储在备份位置。

虽然可与 Windows Server 2012 和更高版本，没有与 Bitlocker 不兼容：

- 执行操作时对齐 VSS 快照，AD 想要执行快照后任务，若要将数据库标记为即将从备份，或在为 RODC 准备 IFM 源的情况下，从数据库中删除凭据。
- HYPER-V 中装入此任务的拍摄卷时, 没有将解锁该卷的未加密访问任何工具。 因此 AD 数据库引擎访问数据库将失败，并最终失败，该快照。

> [!NOTE]
> 受防护的 VM 项目提到以前具有驱动备份作为一个非目标，对于在来宾 VM 的最大的数据保护的 HYPER-V 主机。

## <a name="backup-and-restore-practices-to-avoid"></a>备份和还原要避免的做法

如前文所述，在虚拟机中运行的域控制器具有不适用于在物理计算机中运行的域控制器的限制。 当备份或还原虚拟域控制器时，有某些虚拟化软件功能和做法，不应使用：

   - 请勿复制或克隆 VHD 文件代替执行定期备份域控制器。 如果他 VHD 文件复制或克隆，它会变得陈旧。 然后，如果在正常模式下启动 VHD 时，将会遇到 USN 回滚。 例如，使用 Windows Server Backup 功能，应执行适当的备份操作支持的 Active Directory 域服务 (AD DS)。  
   - 不使用快照功能作为备份来还原已配置为域控制器虚拟机。 复制将出现问题时，还原到以前的状态与 Windows Server 2008 R2 和更低版本的虚拟机。 有关详细信息，请参阅 [USN 和 USN 回滚](#usn-and-usn-rollback)。 尽管使用快照还原只读域控制器 (RODC) 不会导致复制问题，但仍不建议还原此方法。  

## <a name="restoring-a-virtual-domain-controller"></a>还原虚拟域控制器

若要还原的域控制器失败时，必须定期备份系统状态。 系统状态包括 Active Directory 数据和日志文件、 注册表、 系统卷 （SYSVOL 文件夹中） 和操作系统的各种元素。 此要求是相同的物理和虚拟域控制器。 Active Directory 兼容的备份应用程序执行的系统状态还原过程旨在确保在还原过程，包括复制到的通知后的本地和复制 Active Directory 数据库的一致性合作伙伴的调用 ID 重置。 但是，使用托管的虚拟环境和磁盘或操作系统映像应用程序就可以为管理员来跳过检查和还原域控制器系统状态时通常发生的验证。

在域控制器虚拟机出现故障，并更新序列号 (USN) 回滚后未发生，有两种受支持的情况下用于还原虚拟机：

   - 如果存在有效的系统状态数据备份发生故障之前，您可以通过使用备份实用程序，用于创建备份的还原选项还原系统状态。 系统状态数据备份必须已创建的逻辑删除生存期，这是默认情况下，不能超过 180 天范围内使用的 Active Directory – 兼容备份实用程序。 您应备份你的域控制器至少每隔半 tombstone 生存时间。 有关如何确定你的林的特定逻辑删除生存期的说明，请参阅[确定林的 tombstone 生存时间](https://go.microsoft.com/fwlink/?linkid=137177)。  
   - 如果 VHD 文件的工作副本可用，但未提供系统状态备份不可用，则可以删除现有的虚拟机。 使用以前的复制的 vhd 还原现有的虚拟机，但请确保在目录服务还原模式 (DSRM) 启动它和下一节中所述中正确配置注册表。 然后，在正常模式下重新启动域控制器。

如下图中使用该过程来确定还原虚拟化的域控制器的最佳方式。

![](media/virtualized-domain-controller-architecture/Dd363553.85c97481-7b95-4705-92a7-006e48bc29d0(WS.10).gif)

对于 Rodc，还原过程和决策是更简单。

![](media/virtualized-domain-controller-architecture/Dd363553.4c5c5eda-df95-4c6b-84e0-d84661434e5d(WS.10).gif)

## <a name="restoring-the-system-state-backup-of-a-virtual-domain-controller"></a>还原系统状态备份的虚拟域控制器

如果域控制器虚拟机存在有效的系统状态备份，你可安全地将备份还原规定的备份的还原过程用来备份 VHD 文件的工具。

> [!IMPORTANT]
> 若要正确还原域控制器，必须在 dsrm 下启动它。 必须允许在正常模式下启动域控制器。 如果缺少在系统启动进入 DSRM 的机会，将关闭域控制器的虚拟机，才能完全正常模式下启动。 域控制器在 dsrm 下启动其 Usn 以正常模式下启动域控制器因为即使与网络断开连接的域控制器至关重要。 有关 USN 回滚的详细信息，请参阅 USN 和 USN 回滚。 

## <a name="to-restore-the-system-state-backup-of-a-virtual-domain-controller"></a>若要还原系统状态备份的虚拟域控制器

1. 启动域控制器的虚拟机，然后按 f5 键以访问 Windows 启动管理器屏幕。 如果要求您输入连接凭据，立即单击**暂停**按钮在虚拟机上，以便它不会继续启动。 然后，输入您的连接凭据，然后单击**播放**虚拟机上的按钮。 在虚拟机窗口内单击，然后按 F5。

   如果您看不到 Windows 启动管理器屏幕和域控制器开始出现在正常模式下启动，将关闭虚拟机，以防止它完成启动。 重复此步骤中的根据需要多次，直到能够访问 Windows 启动管理器屏幕。 无法从 Windows 错误恢复菜单来访问 DSRM。 因此，关闭虚拟机，然后重试，如果出现 Windows 错误恢复菜单。

2. 在 Windows 启动管理器屏幕中，按 F8 键访问高级的启动选项。
3. 在中**高级启动选项**屏幕上，选择**目录服务还原模式**，然后按 ENTER。 这将在 dsrm 下启动域控制器。
4. 为用于创建系统状态备份的工具使用相应的还原方法。 如果使用 Windows Server Backup，请参阅[执行非权威还原的 AD DS](https://go.microsoft.com/fwlink/?linkid=132637)。

## <a name="restoring-a-virtual-domain-controller-when-an-appropriate-system-state-data-backup-is-not-available"></a>相应的系统状态数据备份不可用时还原的虚拟域控制器

如果没有早于虚拟机失败的系统状态数据备份，可以使用以前的 VHD 文件，若要还原的虚拟机运行的域控制器。 如果可以请 VHD 的副本，以便如果在过程中遇到问题或会错过一个步骤，你可以使用重试复制的 VHD。

> [!IMPORTANT]
> - 不应考虑使用以下过程来替换定期计划内和计划的备份。
> - **使用以下过程执行的还原 Microsoft 不支持，因此应仅当没有其他替代方案。**
> - 如果要还原的 VHD 的副本已在正常模式下启动任何虚拟机，则不使用此过程。

## <a name="to-restore-a-previous-version-of-a-virtual-domain-controller-vhd-without-system-state-data-backup"></a>若要还原以前版本的虚拟域控制器 VHD 没有系统状态数据备份

1. 使用上一个 VHD 在 DSRM 下启动虚拟域控制器，如在上一部分中所述。 不允许在正常模式下启动域控制器。 如果您错过 Windows 启动管理器屏幕和域控制器开始出现在正常模式下启动，将关闭虚拟机，以防止它完成启动。 请参阅上一节中的详细说明输入 DSRM。
2. 打开注册表编辑器。 若要打开注册表编辑器，请单击**启动**，单击**运行**，类型**regedit**，然后单击确定。 如果出现了“用户帐户控制”  对话框，请确认其所显示的操作是你要采取的操作，然后单击“是”  。 在注册表编辑器中，展开以下路径：**HKEY\_本地\_MACHINE\\系统\\CurrentControlSet\\Services\\NTDS\\参数**。 查找名为值**DSA 前一个还原计数**。 如果值存在，请记下的该设置。 如果不存在值，设置为等于默认情况下，为零。 不要添加一个值，如果您没有看到一个存在。
3. 右键单击**参数**键，单击**新建**，然后单击**DWORD （32 位） 值**。
4. 键入新名称**从备份还原数据库**，然后按 ENTER。
5. 双击打开刚刚创建的值**编辑 DWORD （32 位） 值**对话框中，并键入**1**中**值数据**框。 **从备份项还原数据库**在运行 Windows 2000 Server Service Pack 4 (SP4) 中包含的更新的 Windows Server 2003 的域控制器选项才可用[如何检测从 Windows Server 2003、 Windows Server 2008 和 Windows Server 2008 R2 中的 USN 回滚和恢复](https://go.microsoft.com/fwlink/?linkid=137182)安装，Microsoft 知识库和 Windows Server 2008 中。
6. 在正常模式下，重新启动域控制器。
7. 域控制器重新启动后，打开事件查看器。 若要打开事件查看器，请依次单击“开始”  、“控制面板”  ，双击“管理工具”  ，然后双击“事件查看器”  。
8. 展开**应用程序和服务日志**，然后单击**目录服务**日志。 请确保事件出现在详细信息窗格中。
9. 右键单击**Directory Services**记录，然后依次**查找**。 在中**查找内容**，类型**1109年**，然后单击**查找下一个**。
10. 应会看到至少一个事件 ID 1109 条目。 如果您未看到此条目，请转到下一步。 否则为双击该条目，然后查看确认到 InvocationID 进行更新的文本：

    ```
    Active Directory has been restored from backup media, or has been configured to host an application partition. 
    The invocationID attribute for this directory server has been changed. 
    The highest update sequence number at the time the backup was created is <time>

    InvocationID attribute (old value):<Previous InvocationID value>
    InvocationID attribute (new value):<New InvocationID value>
    Update sequence number:<USN>

    The InvocationID is changed when a directory server is restored from backup media or is configured to host a writeable application directory partition.
    ```

11. 关闭事件查看器。
12. 使用注册表编辑器验证中的值**DSA 前一个还原计数**等同于以前的值加 1。 如果这不是正确的值和事件查看器中的事件 ID 1109 中找不到一个条目，请验证域控制器的 service pack 是最新。 您不能尝试执行此过程再次上相同的 VHD。 您可以将 VHD 或尚未启动正常模式下开始操作而在步骤 1 的不同 VHD 的副本上再次尝试。
13. 打开注册表编辑器。

## <a name="usn-and-usn-rollback"></a>USN 和 USN 回滚

本部分介绍可导致不正确使用还原操作的 Active Directory 数据库的较旧版本的虚拟机的复制问题。 有关 Active Directory 复制过程的其他详细信息，请参阅[Active Directory 复制概念](../replication/active-directory-replication-concepts.md)

<!-- Replaced this link with 2016 article: [How the Active Directory Replication Model Works](http://go.microsoft.com/fwlink/?linkid=27636) (http://go.microsoft.com/fwlink/?LinkID=27636). -->

## <a name="usns"></a>Usn

Active Directory 域服务 (AD DS) 使用更新序列号 (Usn) 来跟踪数据的域控制器之间的复制。 更改数据在目录中，每次 USN 都会增加以指示已进行了更改。

目标域控制器可以存储每个目录分区，Usn 用于跟踪的最新发起更新的域控制器引入到其数据库，以及存储的一个副本的每个其他域控制器的状态目录分区。 当域控制器将更改复制到另一个时，这些查询使用 Usn 大于最后一个域控制器从每个合作伙伴收到的更改的 USN 的更改其复制伙伴。

以下两个复制元数据表包含 Usn。 源和目标域控制器使用它们来筛选目标域控制器需要的更新。

1. **最新程度矢量**:目标域控制器维护用于跟踪从所有源域控制器接收的原始更新表。 在目标域控制器请求发生更改时适用于目录分区，它提供了到源域控制器及其最新程度矢量。 然后，源域控制器使用此值来筛选发送到目标域控制器的更新。 源域控制器将其最新程度矢量发送到目标成功复制循环完成时，为了确保目标域控制器，知道它已和每个域控制器的同步原始更新和更新是在与源相同的级别。  
2. **高使用标记**:一个值，目标域控制器维护来跟踪它从特定分区的特定源域控制器接收的最新更改。 高使用标记阻止源域控制器发送出已从其接收由目标域控制器的更改。  

## <a name="directory-database-identity"></a>目录数据库身份识别

Usn，除了域控制器跟踪的源复制伙伴的目录数据库。 从服务器对象本身的标识单独维护的目录数据库服务器上运行的标识。 每个域控制器上的目录数据库标识存储在**invocationID**位于以下的轻型目录访问协议 (LDAP) 路径下的 NTDS 设置对象的属性： cn = NTDS设置、 cn = ServerName，cn = Servers，cn =*SiteName*，cn = Sites，cn = Configuration，dc =*ForestRootDomain*。 服务器对象标识存储在**objectGUID** NTDS 设置对象的属性。 永远不会更改服务器对象的标识。 但是，目录数据库更改的标识系统状态还原过程会在服务器上或添加应用程序目录分区，然后删除，而且更高版本从服务器重新添加。 (另一种方案： 来宾 HyperV 实例触发时包含虚拟 DC 的 VHD 在分区上其 VSS 编写器，进而触发其自身的 VSS 编写器 （使用备份/还原上面的相同机制） 导致的 invocationID 的另一种方法重置）

因此， **invocationID**有效地与一系列发起与目录数据库的特定版本的域控制器上的更新。 最新程度矢量和最高使用标记将标记表使用**invocationID**和即将 DC GUID 分别以便的该域控制器知道从 Active Directory 的一份数据库的复制信息。

**InvocationID**是一个全局唯一标识符 (GUID) 值，在运行该命令后是可见的输出的顶部附近**repadmin /showrepl**。 以下文本表示命令的示例输出：

   ```
   Repadmin: running command /showrepl against full DC local host
   Default-First-Site-Name\VDC1
   DSA Options: IS_GC
   DSA object GUID: 966651f3-a544-461f-9f2c-c30c91d17818
   DSA invocationID: b0d9208b-8eb6-4205-863d-d50801b325a9
   ```

AD DS 正确还原的域控制器上，当**invocationID**重置。 由于此更改，将会在复制流量 – 其持续时间是相对于正在复制的分区的大小增加

例如，假设 VDC1 和 DC2 是同一域中的两个域控制器。 下图显示了有关 VDC1 DC2 的角度来看，在适当的还原情况下重置 invocationID 值时。

![](media/virtualized-domain-controller-architecture/Dd363553.ca71fc12-b484-47fb-991c-5a0b7f516366(WS.10).gif)

## <a name="usn-rollback"></a>USN 回滚

绕过 Usn 的正常更新并尝试使用低于其最新的更新的 USN 的域控制器时，会发生 USN 回滚。 将检测到 USN 回滚和之前创建分歧林中时，在大多数情况下，将停止复制。 

USN 回滚可能会导致在许多方面，例如，使用旧的虚拟硬盘 (VHD) 文件或不能确保该物理计算机会保持脱机状态永久在转换后执行物理到虚拟转换 （P2V 转换） 时。 采取以下预防措施以确保 USN 回滚不会发生：

   - 如果未运行 Windows Server 2012 或更高版本，不需要或使用的域控制器虚拟机快照。
   - 不要复制域控制器 VHD 文件。  
   - 如果未运行 Windows Server 2012 或更高版本，不导出正在运行的域控制器虚拟机。  
   - 不要还原域控制器或尝试回滚由其他任何方式比受支持的备份解决方案，如 Windows Server Backup 的 Active Directory 数据库的内容。  

在某些情况下，可能会发送未检测到的 USN 回滚。 在其他情况下，它可能会导致其他复制错误。 在这些情况下，有必要确定问题的范围并及时地处理它。 有关如何删除可能会导致 USN 回滚的延迟对象的信息，请参阅[过时的 Active Directory 对象在 Windows Server 2003 中生成事件 ID 1988](https://go.microsoft.com/fwlink/?linkid=137185) Microsoft 知识库中。

## <a name="usn-rollback-detection"></a>USN 回滚检测

在大多数情况下，而没有相应的 USN 回退的重置**invocationID**引起的不正确的还原过程检测到。 不正确的域控制器还原操作后，Windows Server 2008 提供了保护，以应对不适当的复制。 这些保护会触发这一事实的不正确的还原操作会导致较低的 Usn，表示发起更改合作伙伴已收到的复制。

在 Windows Server 2008 和 Windows Server 2003 SP1 中，通过使用以前用过 USN，在目标域控制器请求更改时由其源复制伙伴的响应来进行解释要表示其复制的目标域控制器元数据已过时。 这指示，源域控制器上的 Active Directory 数据库已回滚到以前的状态。 例如，虚拟机的 VHD 文件已回滚到以前的版本。 在这种情况下，目标域控制器将启动已标识为具有发生了不正确还原的域控制器上的以下隔离度量值：

   - AD DS 可以暂停 Net Logon 服务，这样可防止用户帐户和计算机帐户更改帐户密码。 此操作可防止丢失此类更改，如果它们不正确的还原后发生。  
   - AD DS 禁用入站和出站 Active Directory 复制。  
   - AD DS 以指示该条件在目录服务事件日志中生成事件 ID 2095。  

下图显示了 VDC2，虚拟机运行的目标域控制器上检测到 USN 回滚时，会发生事件的序列。 在此图中，USN 回滚的检测时，发生 VDC2 上复制伙伴检测到 VDC2 已发送由目标域控制器，这表示 VDC2 的数据库已回滚之前发现最新 USN 值时间不正确。

![](media/virtualized-domain-controller-architecture/Dd363553.373b0504-43fc-40d0-9908-13fdeb7b3f14(WS.10).gif)

如果目录服务事件日志报告事件 ID 2095，立即完成以下过程。

## <a name="to-resolve-event-id2095"></a>若要解决事件 ID 2095

1. 隔离网络中的错误记录在虚拟机。
2. 尝试确定是否源自此域控制器的任何更改，并传播到其他域控制器。 如果事件的快照的结果或正在启动虚拟机的副本，请确定发生 USN 回滚的时间。 然后可以检查域控制器的复制伙伴，以确定是否从那时起发生复制。

   Repadmin 工具可用于做出此判断。 有关如何使用 Repadmin 的信息，请参阅[监视和故障排除 Active Directory 复制使用 Repadmin](https://go.microsoft.com/fwlink/?linkid=122830)。 如果您不能确定自己，请联系[Microsoft 支持部门](https://support.microsoft.com)以获得帮助。

3. 强制降级域控制器。 这涉及到域控制器的元数据清理和占用操作主机 （也称为灵活单主机操作或 FSMO） 角色。 有关详细信息，请参阅的"正在恢复从 USN 回滚"部分[了解如何检测和从 Windows Server 2003、 Windows Server 2008 和 Windows Server 2008 R2 中的 USN 回滚中恢复](https://go.microsoft.com/fwlink/?linkid=137182)Microsoft 知识库中。
4. 删除域控制器以前的所有 VHD 文件。

## <a name="undetected-usn-rollback"></a>未检测到的 USN 回滚

不可能在一个两种情况下检测到 USN 回滚：

1. VHD 文件附加到不同的虚拟机的同时运行多个位置中。  
2. 过去的其他域控制器接收的最后一个 USN 增加了还原的域控制器上的 USN。  

在第一种情况下，其他域控制器可能会使用任何一个虚拟机的复制，而无需复制到其他任一虚拟机上可能会发生更改。 林的此分歧很难检测，并且它将导致不可预知目录响应。 如果在同一网络上运行的物理和虚拟机，P2V 迁移后会出现此情况。 如果多个虚拟域控制器是创建从同一个物理域控制器，然后在同一网络上运行，也可能发生这种情况。

在第二种情况下，范围的 Usn 适用于两个不同的更改集。 可以继续执行此很长时间会被发现。 只要修改在此期间创建的对象，就是检测到延迟对象并将其报告为事件 ID 1988 事件查看器中。 下图显示了如何 USN 回滚可能无法检测到在这种情况下。

![](media/virtualized-domain-controller-architecture/Dd363553.63565fe0-d970-4b4e-b5f3-9c76bc77e2d4(WS.10).gif)

## <a name="read-only-domain-controllers"></a>只读域控制器

Rodc 是域控制器承载 Active Directory 数据库中的分区的只读副本。 Rodc 避免大多数 USN 回退问题，因为它们不复制到其他域控制器的任何更改。 但是，如果从已受到 USN 回滚的可写域控制器复制 RODC，RODC 也会影响。

不建议还原 RODC 使用的快照。 还原使用 Active Directory – 兼容备份应用程序的 RODC。 此外，与可写域控制器一样，必须格外小心以不允许 RODC 脱机时间超过逻辑删除生存期。 这种情况可能会导致在 RODC 上的延迟对象。

有关 Rodc 的详细信息，请参阅[只读域控制器计划和部署指南](../../deploy/rodc/read-only-domain-controller-updates.md)。
