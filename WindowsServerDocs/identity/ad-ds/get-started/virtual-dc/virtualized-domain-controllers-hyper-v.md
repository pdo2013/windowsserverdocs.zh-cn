---
title: 使用 Hyper-v 虚拟化域控制器
description: 在 Hyper-v 中虚拟化 Windows Server Active Directory 域控制器时的注意事项
author: MicrosoftGuyJFlo
ms.author: joflore
ms.date: 04/19/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.openlocfilehash: 491f4f2e2526e7cff024779ee3ecf9f771e64af4
ms.sourcegitcommit: 23a6e83b688119c9357262b6815c9402c2965472
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/16/2019
ms.locfileid: "69560576"
---
# <a name="virtualizing-domain-controllers-using-hyper-v"></a>使用 Hyper-v 虚拟化域控制器

> 适用于：Windows Server 2016

为了使指南适用于 Windows Server 2016, 将更新本主题。 Windows Server 2012 对虚拟化域控制器 (Dc) 进行了很多改进, 包括防止虚拟 Dc 上的 USN 回滚的保护功能, 以及克隆虚拟 Dc 的功能。 有关这些改进的详细信息, 请参阅[Active Directory 域服务 (AD DS) 虚拟化简介 (等级 100)](../../introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100.md)。

Hyper-v 将不同的服务器角色合并到单台物理计算机上。 本指南介绍如何将域控制器作为32位或64位来宾操作系统运行。

## <a name="planning-to-virtualize-domain-controllers"></a>规划虚拟化域控制器

本部分介绍 Hyper-v 服务器的硬件要求, 如何避免单点故障, 为 Hyper-v 服务器和虚拟机选择适当的配置类型, 以及安全和性能决策。

## <a name="hyper-v-requirements"></a>Hyper-v 要求

若要安装和使用 Hyper-v 角色, 必须具备以下各项:

   - **X64 处理器**
      - Hyper-v 在 Windows Server 2008 或更高版本的基于 x64 的版本中可用。  
   - **硬件辅助虚拟化**
      - 此功能在包含虚拟化选项的处理器 (特别是 Intel 虚拟化技术 (Intel VT) 或 AMD 虚拟化 (AMD)) 中提供。  
   - **硬件数据执行保护 (DEP)**
      - 硬件 DEP 必须可用且已启用。 具体来说, 您必须启用 Intel XD 位 (执行禁用位) 或 AMD NX 位 (无执行位)。  

## <a name="avoid-creating-single-points-of-failure"></a>避免创建单点故障

你应尝试避免在规划虚拟域控制器部署时产生潜在的单点故障。 可以通过实现系统冗余来避免引入潜在的单点故障。 例如, 请考虑以下建议, 同时记住管理成本的潜在增加:

1. 在不同的虚拟化主机上至少为每个域运行两个虚拟化域控制器, 这可降低在单个虚拟化主机发生故障时丢失所有域控制器的风险。  
2. 如建议使用其他技术, 请多元化运行域控制器的硬件 (使用不同 Cpu、主板、网络适配器或其他硬件)。 硬件 diversification 限制了因特定于供应商配置、驱动程序或硬件类型的故障而造成的损坏。  
3. 如果可能, 域控制器应在位于世界不同地区的硬件上运行。 这有助于降低影响托管域控制器的站点的灾难或故障的影响。  
4. 维护每个域中的物理域控制器。 这可降低虚拟化平台故障的风险, 这会影响使用该平台的所有主机系统。  

## <a name="security-considerations"></a>安全注意事项

运行虚拟域控制器的主机必须被视为可写域控制器 (即使该计算机只是加入域的计算机或工作组计算机) 的管理。 这是一个重要的安全注意事项。 Mismanaged 主机容易遭受特权提升攻击, 这种攻击会在恶意用户获得未经授权或合法分配的访问权限和系统特权时出现。 恶意用户可以使用这种类型的攻击来破坏此计算机托管的所有虚拟机、域和林。

当你计划虚拟化域控制器时, 请确保记住以下安全注意事项:

   - 托管虚拟、可写域控制器的计算机的本地管理员应将凭据中的等效项视为该域控制器所属的所有域和林的默认域管理员。  
   - 避免安全和性能问题的建议配置是运行 Windows Server 2008 或更高版本的服务器核心安装的主机, 而不是 Hyper-v 以外的应用程序。 此配置限制了服务器上安装的应用程序和服务的数量, 这将导致更高的性能和更少的应用程序和服务受到恶意攻击, 从而攻击计算机或网络。 这种类型的配置的效果称为减少的攻击面。 在不能令人满意的分支机构或其他位置, 建议使用只读域控制器 (RODC)。 如果存在单独的管理网络, 我们建议仅将主机连接到管理网络。  
   - 你可以将 Bitlocker 用于域控制器, 因为 Windows Server 2016 你可以使用虚拟 TPM 功能, 还可以提供来宾密钥材料来解锁系统卷。
   - [受保护的构造和受防护的 vm](/it-server/WindowsServerDocs/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md)可以提供额外的控制来保护域控制器。

有关 Rodc 的详细信息, 请参阅[只读域控制器规划和部署指南](../../deploy/rodc/read-only-domain-controller-updates.md)。

有关保护域控制器的详细信息, 请参阅[保护 Active Directory 安装的最佳实践指南](../../plan/security-best-practices/best-practices-for-securing-active-directory.md)。

## <a name="security-boundaries-for-different-host-and-guest-configurations"></a>不同主机和来宾配置的安全边界

使用虚拟机, 可以具有多个不同的域控制器配置。 请仔细考虑虚拟机影响 Active Directory 拓扑中边界和信任的方式。 下表介绍了 Active Directory 域控制器和主机 (Hyper-v 服务器) 及其来宾计算机 (在 Hyper-v 服务器上运行的虚拟机) 的可能配置。

|Machine|配置1|Configuration 2|
|-------|---------------|---------------|
|主机|工作组或成员计算机|工作组或成员计算机|
|Guest|域控制器|工作组或成员计算机|

![](media/virtualized-domain-controller-architecture/Dd363553.f44706fd-317e-4f0b-9578-4243f4db225f(WS.10).gif)

   - 主计算机上的管理员与可写域控制器来宾上的域管理员具有相同的访问权限, 必须将其视为。 对于 RODC 来宾, 主计算机的管理员与来宾 RODC 上的本地管理员具有相同的访问权限。   
   - 如果主机加入到同一个域中, 则虚拟机中的域控制器在主机上具有管理权限。 如果恶意用户第一次获得虚拟机1的访问权限, 则恶意用户可能会破坏所有虚拟机。 这称为攻击向量。 如果有多个域或林的域控制器, 则这些域应具有集中管理, 其中一个域的管理员在所有域上都是受信任的。  
   - 即使将虚拟机1作为 RODC 安装, 也存在从虚拟机1进行攻击的机会。 尽管 RODC 的管理员没有明确具有域管理员权限, 但可以使用 RODC 将策略发送到主计算机。 这些策略可能包括启动脚本。 如果此操作成功, 主机可能会受到损害, 然后可以使用该计算机来损害主机上的其他虚拟机。  

## <a name="security-of-vhd-files"></a>VHD 文件的安全性

虚拟域控制器的 VHD 文件等效于物理域控制器的物理硬盘驱动器。 这种情况下, 应使用与保护物理域控制器硬盘驱动器相同的小心来保护它。 请确保仅允许可靠和受信任的管理员访问域控制器的 VHD 文件。

## <a name="rodcs"></a>Rodc

Rodc 的优点之一是能够将其放在无法保证物理安全的位置, 例如分支机构。 你可以使用 Windows BitLocker 驱动器加密来保护 VHD 文件本身 (而不是其中的文件系统), 以免因盗窃物理磁盘而在主机上泄露。 

## <a name="performance"></a>性能

在新的微内核64位体系结构中, 以前的虚拟化平台的 Hyper-v 性能显著增加了。 为了获得最佳主机性能, 主机应为 Windows Server 2008 或更高版本的服务器核心安装, 并且不应安装 Hyper-v 以外的其他服务器角色。

虚拟机的性能特别依赖于工作负荷。 为了保证 Active Directory 性能, 请测试特定拓扑。 使用工具 (如可靠性和性能监视器 (Perfmon) 或[Microsoft 评估和计划 (MAP) 工具包](https://go.microsoft.com/fwlink/?linkid=137077)) 在一段时间内评估当前工作负荷。 如果要对当前在网络中存在的所有服务器和服务器角色进行清点, 还可以使用地图工具。

若要大致了解虚拟化域控制器的性能, 请使用[Active Directory 性能测试工具 (ADTest)](https://go.microsoft.com/fwlink/?linkid=137088)执行以下性能测试。

轻型目录访问协议 (LDAP) 测试在具有 ADTest 的物理域控制器上运行, 然后在与物理域控制器相同的服务器上托管的虚拟机上运行。 物理计算机只使用了一个逻辑处理器, 且虚拟机只使用了一个虚拟处理器来轻松达到 100% 的 CPU 使用率。 在下表中, 每个测试后, 括号中的字母和数字表示 ADTest 中的特定测试。 如此数据所示, 虚拟化域控制器的性能为88到 98% 的物理域控制器性能。

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
<th>测量</th>
<th>测试</th>
<th>物理</th>
<th>虚拟主机</th>
<th>Delta</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>搜索数/秒</p></td>
<td><p>搜索基本范围内的公用名 (L1)</p></td>
<td><p>11508</p></td>
<td><p>10276</p></td>
<td><p>-10.71%</p></td>
</tr>
<tr class="even">
<td><p>搜索数/秒</p></td>
<td><p>在基本范围 (L2) 中搜索一组属性</p></td>
<td><p>10123</p></td>
<td><p>9005</p></td>
<td><p>-11.04%</p></td>
</tr>
<tr class="odd">
<td><p>搜索数/秒</p></td>
<td><p>搜索基本范围 (L3) 中的所有属性</p></td>
<td><p>1284</p></td>
<td><p>1242</p></td>
<td><p>-3.27%</p></td>
</tr>
<tr class="even">
<td><p>搜索数/秒</p></td>
<td><p>在子树范围 (L6) 中搜索公用名</p></td>
<td><p>8613</p></td>
<td><p>7904</p></td>
<td><p>-8.23%</p></td>
</tr>
<tr class="odd">
<td><p>每秒成功绑定数</p></td>
<td><p>执行快速绑定 (B1)</p></td>
<td><p>1438</p></td>
<td><p>1374</p></td>
<td><p>-4.45%</p></td>
</tr>
<tr class="even">
<td><p>每秒成功绑定数</p></td>
<td><p>执行简单绑定 (B2)</p></td>
<td><p>611</p></td>
<td><p>550</p></td>
<td><p>-9.98%</p></td>
</tr>
<tr class="odd">
<td><p>每秒成功绑定数</p></td>
<td><p>使用 NTLM 执行绑定 (B5)</p></td>
<td><p>1068</p></td>
<td><p>1056</p></td>
<td><p>-1.12%</p></td>
</tr>
<tr class="even">
<td><p>写入数/秒</p></td>
<td><p>写入多个属性 (W2)</p></td>
<td><p>6467</p></td>
<td><p>5885</p></td>
<td><p>-9.00%</p></td>
</tr>
</tbody>
</table>

为了确保性能满意, 已安装集成组件 (IC), 以允许来宾操作系统使用 "自旋" 或虚拟机监控程序感知的综合驱动程序。 在安装过程中, 可能需要使用模拟集成驱动电子设备 (IDE) 或网络适配器驱动程序。 在生产环境中, 应将这些模拟驱动程序替换为综合驱动程序以提高性能。

使用可靠性和性能管理器 (Perfmon) 监视虚拟机的性能时, 在虚拟机中, CPU 信息将不会完全准确, 这是因为虚拟 CPU 在物理处理器上的计划方式。 如果要获取在 Hyper-v 服务器上运行的虚拟机的 CPU 信息, 请使用主机分区中的 Hyper-v 虚拟机监控程序逻辑处理器计数器。

有关 AD DS 和 Hyper-v 性能优化的详细信息, 请参阅[Windows Server 2016 的性能优化指南](../../../../administration/performance-tuning/index.md)。

此外, 请不要计划在配置为域控制器的虚拟机上使用差异磁盘 VHD, 因为差异磁盘 VHD 会降低性能。 若要了解有关 Hyper-v 磁盘类型 (包括差异磁盘) 的详细信息, 请参阅[新建虚拟硬盘向导](http://go.microsoft.com/fwlink/?linkid=137279)。

有关虚拟宿主环境中 AD DS 的其他信息, 请参阅在 Microsoft 知识库中的[虚拟宿主环境中承载 Active Directory 域控制器时要注意的事项](https://go.microsoft.com/fwlink/?linkid=141292)。

## <a name="deployment-considerations-for-virtualized-domain-controllers"></a>虚拟化域控制器的部署注意事项

在部署域控制器时, 有几种常见的虚拟机实践需要避免, 并需要特别注意时间同步和存储。

## <a name="virtualization-deployment-practices-to-avoid"></a>要避免的虚拟化部署实践

虚拟化平台 (如 Hyper-v) 提供了许多便利功能, 可更轻松地管理、维护、备份和迁移计算机。 但是, 不应将以下常见的部署实践和功能用于虚拟域控制器:

- 若要确保 Active Directory 写入的持久性, 请不要部署虚拟域控制器的数据库文件 (Active Directory 数据库 (NTDS)。DIT)、日志和 SYSVOL。 相反, 请创建附加到虚拟 SCSI 控制器的第二个 VHD, 并确保在安装域控制器期间将数据库、日志和 SYSVOL 放置在虚拟机的 SCSI 磁盘上。  
- 不要在配置为域控制器的虚拟机上实现差异磁盘虚拟硬盘 (Vhd)。 这使得还原到以前的版本变得很容易, 并且还会降低性能。 有关 VHD 类型的详细信息, 请参阅[新建虚拟硬盘向导](https://go.microsoft.com/fwlink/?linkid=137279)。  
- 不要在未首先使用系统准备工具 (Sysprep) 准备的 Windows Server 操作系统的副本上部署新的 Active Directory 域和林。 有关运行 Sysprep 的详细信息, 请参阅[sysprep (系统准备) 概述](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview)

   > [!WARNING]
   > 不支持在域控制器上运行 Sysprep。

- 若要帮助防止潜在的更新序列号 (USN) 回滚情况, 请不要使用表示已部署的域控制器的 VHD 文件的副本来部署其他域控制器。 有关 USN 回滚的详细信息，请参阅 [USN 和 USN 回滚](#usn-and-usn-rollback)。
   - Windows Server 2012 和更高版本允许管理员在需要部署其他域控制器时, 克隆域控制器映像
- 不要使用 Hyper-v 导出功能导出运行域控制器的虚拟机。
  - 对于 Windows Server 2012 和更高版本, 域控制器虚拟来宾的导出和导入处理方式类似于非权威还原, 因为它检测到对代 ID 的更改, 但未将其配置为进行克隆。
  - 确保你未使用已导出的来宾。
    - 你可以使用 Hyper-v 复制来保留域控制器的第二个非活动副本。 如果启动复制的映像, 还需要执行正确的清理, 因为在导出 DC 来宾映像后不使用源。

## <a name="physical-to-virtual-migration"></a>物理到虚拟迁移

System Center Virtual Machine Manager (VMM) 2008 提供对物理计算机和虚拟机的统一管理。 它还提供将物理计算机迁移到虚拟机的功能。 此过程称为物理到虚拟机转换 (P2V 转换)。 在 P2V 转换过程中, 要迁移的新虚拟机和物理域控制器不得同时运行, 以避免在[usn 和 Usn 回滚](#usn-and-usn-rollback)中所述的 usn 回滚情况。

应使用脱机模式执行 P2V 转换, 以便在域控制器重新打开时目录数据保持一致。 在转换物理服务器向导中提供并建议使用 "脱机模式" 选项。 有关联机模式和脱机模式之间差异的说明, 请参阅[P2V:在 VMM 中将物理计算机转换为虚拟机](https://go.microsoft.com/fwlink/?linkid=155072)。 在 P2V 转换期间, 虚拟机不应连接到网络。 只有在完成并验证了 P2V 转换过程之后, 才应启用虚拟机的网络适配器。 此时, 物理源计算机将处于关闭状态。 重新格式化硬盘之前, 不要再次将物理源计算机重新进入网络。

> [!NOTE]
> 有一些更安全的选项可用于创建新的虚拟 Dc, 而不会运行创建 USN 回滚的风险。 你可以通过定期升级设置新的虚拟 DC, 从介质安装 (IfM) 升级, 还可以使用域控制器克隆 (如果你已经有至少一个虚拟 DC)。
这也有助于避免与硬件或平台相关的问题的问题。

> [!WARNING]
> 若要防止 Active Directory 复制出现问题, 请确保在任意时间点在给定的网络中仅存在给定域控制器的一个实例 (物理或虚拟)。
> 您可以降低旧克隆出现问题的可能性:
> 
> - 当新虚拟 DC 正在运行时, 使用: netdom resetpwd/Server: < 域控制器 > 来更改计算机帐户密码两次 。
> - 导出和导入新的虚拟来宾, 使其成为新的代 ID, 进而成为数据库调用 ID。
> 

## <a name="using-p2v-migration-to-create-test-environments"></a>使用 P2V 迁移创建测试环境

可以通过 VMM 使用 P2V 迁移来创建测试环境。 你可以将生产域控制器从物理计算机迁移到虚拟机, 以创建测试环境, 而无需永久性关闭生产域控制器。 但是, 如果存在相同域控制器的两个实例, 则测试环境必须位于与生产环境不同的网络中。 在创建包含 P2V 迁移的测试环境时, 必须格外小心, 以免发生可能影响测试环境和生产环境的 USN 回滚。 下面是一种方法, 可以使用它通过 P2V 创建测试环境。

根据 "物理到虚拟迁移" 部分中所述的准则, 将每个域中的一个就地域控制器迁移到使用 P2V 的测试虚拟机。 物理生产计算机和测试虚拟机在恢复联机状态时必须处于不同的网络中。 若要避免在测试环境中回滚 USN, 必须使要从物理计算机迁移到虚拟机的所有域控制器都处于脱机状态。 (可通过停止 ntds 服务或以目录服务还原模式 (DSRM) 重新启动计算机来完成此操作。)域控制器脱机后, 不应向环境引入任何新的更新。 在 P2V 迁移期间, 计算机必须保持脱机状态;在完全迁移所有计算机之前, 不应将任何计算机恢复为联机状态。 若要了解有关 USN 回滚的详细信息, 请参阅 USN 和 USN 回滚。

后续的测试域控制器应在测试环境中升级为副本。

## <a name="time-service"></a>时间服务

对于配置为域控制器的虚拟机, 建议你在充当域控制器的主机系统和来宾操作系统之间禁用时间同步。 这样, 来宾域控制器就可以同步域层次结构中的时间。

若要禁用 Hyper-v 时间同步提供程序, 请关闭 VM, 并在 "Integration Services" 下清除 "时间同步" 复选框。

> [!NOTE]
> 本指南最近已更新, 以反映从仅域层次结构同步来宾域控制器的当前建议, 而不是在主机之间部分禁用时间同步的前一建议系统和来宾域控制器。

## <a name="storage"></a>存储

若要优化域控制器虚拟机的性能并确保 Active Directory 写入的持续性, 请使用以下建议来存储操作系统、Active Directory 和 VHD 文件:

- **来宾存储**。 将 Active Directory 数据库文件 (ntds.dit)、日志文件和 SYSVOL 文件存储在与操作系统文件不同的虚拟磁盘上。 创建连接到虚拟 SCSI 控制器的第二个 VHD, 并将数据库、日志和 SYSVOL 存储在虚拟机的虚拟 SCSI 磁盘上。 与虚拟 IDE 相比, 虚拟 SCSI 磁盘的性能更高, 并且支持强制单元访问 (FUA)。 FUA 可确保操作系统直接从介质写入和读取数据, 绕过任何和所有缓存机制。

  > [!NOTE]
  > 如果打算将 Bitlocker 用于虚拟 DC 来宾, 则需要确保将其他卷配置为使用 "自动解锁"。
  > 有关配置自动解锁的详细信息, 请参阅[BitLockerAutoUnlock](https://docs.microsoft.com/powershell/module/bitlocker/enable-bitlockerautounlock)

- **VHD 文件的主机存储**。 针对主机存储建议解决 VHD 文件的存储。 为了获得最佳性能, 请不要将 VHD 文件存储在由其他服务或应用程序使用的磁盘上, 如安装了主机 Windows 操作系统的系统磁盘。 将每个 VHD 文件存储在独立于主机操作系统和任何其他 VHD 文件的分区中。 理想的配置是将每个 VHD 文件存储在单独的物理驱动器上。  

  主机物理磁盘系统还必须至少满足以下条件**之一**, 以满足虚拟化工作负荷数据完整性的要求:  

   - 系统使用服务器级磁盘 (SCSI、光纤通道)。  
   - 系统确保磁盘连接到支持电池的缓存主机总线适配器 (HBA)。  
   - 系统使用存储控制器 (例如 RAID 系统) 作为存储设备。  
   - 系统确保磁盘的电源受到不间断电源 (UPS) 的保护。  
   - 系统确保磁盘的写入缓存功能处于禁用状态。  

- **修复了 VHD 和传递磁盘**。 有多种方法可以为虚拟机配置存储。 使用 VHD 文件时, 固定大小的 Vhd 比动态 Vhd 更高效, 因为固定大小的 Vhd 的内存是在创建时分配的。 传递磁盘 (虚拟机可用于访问物理存储媒体) 更好地针对性能进行了优化。 传递磁盘实质上是连接到虚拟机的物理磁盘或逻辑单元号 (Lun)。 传递磁盘不支持快照功能。 因此, 传递磁盘是首选的硬盘配置, 因为不建议将快照用于域控制器。  

若要减少损坏 Active Directory 数据的可能性, 请使用虚拟 SCSI 控制器:

   - 在托管虚拟域控制器的 Hyper-v 服务器上使用 SCSI 物理驱动器 (而非 IDE/ata 驱动器)。 如果无法使用 SCSI 驱动器, 请确保在托管虚拟域控制器的 ATA/IDE 驱动器上禁用写缓存。 有关详细信息, 请参阅[事件 ID 1539 –数据库完整性](https://go.microsoft.com/fwlink/?linkid=162419)。
   - 为了保证 Active Directory 写入的持久性, 必须将 Active Directory 数据库、日志和 SYSVOL 置于虚拟 SCSI 磁盘上。 虚拟 SCSI 磁盘支持强制单元访问 (FUA)。 FUA 可确保操作系统直接从介质写入和读取数据, 绕过任何和所有缓存机制。  

## <a name="operational-considerations-for-virtualized-domain-controllers"></a>虚拟化域控制器的操作注意事项

在虚拟机上运行的域控制器的操作限制不适用于在物理计算机上运行的域控制器。 使用虚拟化域控制器时, 某些虚拟化软件功能和做法不应使用:

   - 请勿暂停、停止或存储虚拟机中域控制器的已保存状态, 时间段比林的逻辑删除生存期长, 然后从暂停或保存状态恢复。 这样做可能会影响复制。 若要了解如何确定林的逻辑删除生存期, 请参阅[确定林的逻辑删除生存期](https://go.microsoft.com/fwlink/?linkid=137177)。  
   - 请勿复制或克隆虚拟硬盘 (Vhd)。 即使具有来宾 VM 的安全措施, 也仍然可以复制单个 Vhd 并导致 USN 回滚。
   - 不要拍摄或使用虚拟域控制器的快照。 Windows Server 2012 和更高版本在技术上是受支持的, 它不能代替良好的备份策略。 获取 DC 快照或还原快照的原因有很多。
   - 不要在配置为域控制器的虚拟机上使用差异磁盘 VHD。 这会使还原到以前的版本非常简单, 并且还会降低性能。  
   - 不要在运行域控制器的虚拟机上使用导出功能。  
   - 不要还原域控制器, 或者尝试通过除使用受支持的备份以外的任何方式来回滚 Active Directory 数据库的内容。 有关详细信息, 请参阅[虚拟化域控制器的备份和还原注意事项](#backup-and-restore-practices-to-avoid)。  

所有这些建议均可帮助避免更新序列号 (USN) 回滚。 有关 USN 回滚的详细信息, 请参阅 USN 和 USN 回滚。

## <a name="backup-and-restore-considerations-for-virtualized-domain-controllers"></a>虚拟化域控制器的备份和还原注意事项

备份域控制器是任何环境的关键要求。 如果出现域控制器故障或管理错误, 备份将防止数据丢失。 如果发生此类事件, 则必须将域控制器的系统状态回滚到故障或错误之前的某个时间点。 将域控制器还原到正常状态的支持方法是使用 Active Directory 兼容的备份应用程序 (如 Windows Server 备份) 来还原源自域的当前安装的系统状态备份。控制器. 有关将 Windows Server 备份与 Active Directory 域服务 (AD DS) 结合使用的详细信息, 请参阅[AD DS 备份和恢复循序渐进指南](https://go.microsoft.com/fwlink/?LinkId=138501)。

利用虚拟机技术, Active Directory 还原操作的某些要求会提高重要性。 例如, 如果使用虚拟硬盘 (VHD) 文件的副本还原域控制器, 则在还原域控制器的数据库版本后, 你将绕过更新其数据库版本的关键步骤。 复制将使用不适当的跟踪号继续, 导致域控制器副本之间出现不一致的数据库。 在大多数情况下, 此问题会被复制系统检测到, 但不会报告任何错误, 这两个域控制器之间的不一致。

有一种受支持的方法来执行虚拟化域控制器的备份和还原:

1. 在来宾操作系统中运行 Windows Server 备份。  

使用 Windows Server 2012 和更新版本的 Hyper-v 主机和来宾, 你可以使用快照、来宾 VM 导出和导入以及 Hyper-v 复制来支持域控制器的备份。 但是, 所有这些都不适合用于创建适当的备份历史记录, 但对于来宾 VM 导出稍微例外。

对于 Windows Server 2016 Hyper-v, 支持 "生产快照", 其中, Hyper-v 服务器会触发来宾的基于 VSS 的备份, 并且在使用快照完成来宾时, 主机会提取 Vhd 并将其存储在备份位置。

虽然这适用于 Windows Server 2012 和更高版本, 但与 Bitlocker 不兼容:

- 当执行 VSS 管理单元时, AD 希望执行快照后任务将数据库标记为来自备份, 或在为 RODC 准备 IFM 源的情况下, 从数据库中删除凭据。
- 当 Hyper-v 为此任务装入快照卷时, 没有可用于解锁卷以进行未加密访问的工具。 因此, AD 数据库引擎无法访问数据库, 最终会使快照失败。

> [!NOTE]
> 前面提到的受防护的 VM 项目将 Hyper-v 主机驱动的备份作为来宾 VM 的最大数据保护的非目标。

## <a name="backup-and-restore-practices-to-avoid"></a>要避免的备份和还原做法

如前所述, 在虚拟机中运行的域控制器的限制不适用于在物理计算机上运行的域控制器。 在备份或还原虚拟域控制器时, 某些虚拟化软件功能和做法不应使用:

   - 请勿复制或克隆域控制器的 VHD 文件, 而不是执行定期备份。 如果复制或克隆了 VHD 文件, 则该文件将过时。 然后, 如果 VHD 在正常模式下启动, 则会遇到 USN 回滚。 应 Active Directory 域服务 (AD DS) 支持的备份操作正常执行, 如使用 Windows Server 备份功能。  
   - 不要使用快照功能作为备份来还原配置为域控制器的虚拟机。 当你将虚拟机恢复到 Windows Server 2008 R2 及更早版本时, 复制将会出现问题。 有关详细信息，请参阅 [USN 和 USN 回滚](#usn-and-usn-rollback)。 尽管使用快照还原只读域控制器 (RODC) 不会导致复制问题, 但仍不建议使用此还原方法。  

## <a name="restoring-a-virtual-domain-controller"></a>还原虚拟域控制器

若要在域控制器出现故障时对其进行还原, 必须定期备份系统状态。 系统状态包括 Active Directory 的数据和日志文件、注册表、系统卷 (SYSVOL 文件夹) 以及操作系统的各种元素。 此要求对于物理域控制器和虚拟域控制器都是相同的。 与 Active Directory 兼容的备份应用程序执行的系统状态还原过程旨在确保在还原过程 (包括复制通知) 后本地和复制 Active Directory 数据库的一致性调用 ID 的合作伙伴将重置。 但是, 使用虚拟宿主环境和磁盘或操作系统映像应用程序, 管理员可以绕过域控制器系统状态还原时通常会发生的检查和验证。

如果域控制器虚拟机出现故障, 并且未发生更新序列号 (USN) 回滚, 则还原虚拟机的情况有两种:

   - 如果存在早于故障的有效系统状态数据备份, 则可以使用用于创建备份的备份实用程序的还原选项还原系统状态。 系统状态数据备份必须在 tombstone 生存时间 (默认情况下不超过180天) 内使用与 Active Directory 兼容的备份实用程序创建。 应至少每半个逻辑删除生存期备份域控制器。 有关如何确定林的特定逻辑删除生存期的说明, 请参阅[确定林的逻辑删除生存期](https://go.microsoft.com/fwlink/?linkid=137177)。  
   - 如果 VHD 文件的工作副本可用, 但没有可用的系统状态备份, 则可以删除现有的虚拟机。 使用 VHD 以前的副本还原现有的虚拟机, 但请确保在目录服务还原模式 (DSRM) 中启动该虚拟机, 并正确配置注册表, 如以下部分所述。 然后, 在正常模式下重新启动域控制器。

使用下图中的过程来确定还原虚拟化域控制器的最佳方式。

![](media/virtualized-domain-controller-architecture/Dd363553.85c97481-7b95-4705-92a7-006e48bc29d0(WS.10).gif)

对于 Rodc, 还原过程和决策更为简单。

![](media/virtualized-domain-controller-architecture/Dd363553.4c5c5eda-df95-4c6b-84e0-d84661434e5d(WS.10).gif)

## <a name="restoring-the-system-state-backup-of-a-virtual-domain-controller"></a>还原虚拟域控制器的系统状态备份

如果域控制器虚拟机存在有效的系统状态备份, 则可以按照备份工具 (用于备份 VHD 文件) 所规定的还原过程来安全地还原备份。

> [!IMPORTANT]
> 若要正确还原域控制器, 必须在 DSRM 中启动它。 不得允许域控制器在正常模式下启动。 如果在系统启动期间错过了进入 DSRM 的机会, 请先关闭域控制器的虚拟机, 然后才能在正常模式下将其完全启动。 必须在 DSRM 中启动域控制器, 因为在正常模式下启动域控制器会递增其 Usn, 即使域控制器已与网络断开连接也是如此。 有关 USN 回滚的详细信息, 请参阅 USN 和 USN 回滚。 

## <a name="to-restore-the-system-state-backup-of-a-virtual-domain-controller"></a>还原虚拟域控制器的系统状态备份

1. 启动域控制器的虚拟机, 并按 F5 访问 Windows 启动管理器屏幕。 如果需要输入连接凭据, 请立即单击虚拟机上的 "**暂停**" 按钮, 使其不会继续启动。 然后, 输入连接凭据, 并单击虚拟机上的 "**播放**" 按钮。 在虚拟机窗口中单击, 并按 F5。

   如果看不到 Windows 启动管理器屏幕, 并且域控制器开始在正常模式下启动, 请关闭虚拟机以防止其完成启动。 根据需要重复此步骤多次, 直到可以访问 Windows 启动管理器屏幕。 不能从 Windows 错误恢复菜单访问 DSRM。 因此, 请关闭虚拟机, 并在出现 Windows 错误恢复菜单时重试。

2. 在 Windows 启动管理器屏幕中, 按 F8 访问高级启动选项。
3. 在 "**高级启动选项**" 屏幕中, 选择 "**目录服务还原模式**", 然后按 enter。 这会在 DSRM 中启动域控制器。
4. 使用用于创建系统状态备份的工具的相应还原方法。 如果使用 Windows Server 备份, 请参阅[执行 AD DS 的非权威还原](https://go.microsoft.com/fwlink/?linkid=132637)。

## <a name="restoring-a-virtual-domain-controller-when-an-appropriate-system-state-data-backup-is-not-available"></a>当相应的系统状态数据备份不可用时还原虚拟域控制器

如果你没有早于虚拟机失败的系统状态数据备份, 则可以使用以前的 VHD 文件来还原虚拟机上运行的域控制器。 如果可以, 创建 VHD 的副本, 以便在过程中遇到问题或错过某个步骤时, 可以使用复制的 VHD 重试。

> [!IMPORTANT]
> - 不应考虑使用以下过程作为定期计划备份和计划备份的替代方法。
> - **使用以下过程执行的还原不受 Microsoft 支持, 只应在没有其他替代项时使用。**
> - 如果要还原的 VHD 副本已由任何虚拟机在正常模式下启动, 则不要使用此过程。

## <a name="to-restore-a-previous-version-of-a-virtual-domain-controller-vhd-without-system-state-data-backup"></a>还原以前版本的虚拟域控制器 VHD 而不进行系统状态数据备份

1. 使用以前的 VHD, 如前一部分中所述, 在 DSRM 中启动虚拟域控制器。 不允许域控制器在正常模式下启动。 如果错过了 Windows 启动管理器屏幕, 并且域控制器开始在正常模式下启动, 请关闭虚拟机以防止其完成启动。 有关输入 DSRM 的详细说明, 请参阅上一节。
2. 打开注册表编辑器。 若要打开注册表编辑器, 请单击 "**开始**", 再单击 "**运行**", 键入**regedit**, 然后单击 "确定"。 如果出现了“用户帐户控制”对话框，请确认其所显示的操作是你要采取的操作，然后单击“是”。 在注册表编辑器中, 展开以下路径:**HKEY\_本地计算机系统\\CurrentControlSet Services NTDS参数\\。\\\\\_\\** 查找名为 " **DSA 先前还原计数**" 的值。 如果值为, 请记下该设置。 如果值不存在, 则设置等于默认值0。 如果看不到任何值, 请不要添加值。
3. 右键单击**Parameters**项, 单击 "**新建**", 然后单击 " **DWORD (32 位) 值**"。
4. 键入 "**从备份中还原**的新名称数据库", 然后按 enter。
5. 双击刚创建的值以打开 "**编辑 DWORD (32 位) 值**" 对话框, 然后在 "**值数据**" 框中键入**1** 。 使用 "**从备份中还原的数据库**" 选项在运行带有 Service Pack 4 (SP4) 的 Windows 2000 Server 的域控制器上提供, Windows Server 2003 的更新包括在[Microsoft 知识库中的 windows Server 2003、Windows Server 2008 和 Windows Server 2008 R2](https://go.microsoft.com/fwlink/?linkid=137182)以及 Windows server 2008。
6. 在正常模式下重新启动域控制器。
7. 当域控制器重新启动时, 打开事件查看器。 若要打开事件查看器，请依次单击“开始”、“控制面板”，双击“管理工具”，然后双击“事件查看器”。
8. 展开 "**应用程序和服务日志**", 然后单击 "**目录服务**" 日志。 确保 "详细信息" 窗格中显示事件。
9. 右键单击 "**目录服务**" 日志, 然后单击 "**查找**"。 在 "**查找内容**" 中, 键入**1109**, 然后单击 "**查找下一个**"。
10. 应该会看到至少一个事件 ID 1109 条目。 如果看不到此条目, 请转到下一步。 否则, 请双击该条目, 然后查看确认已对 InvocationID 进行更新的文本:

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
12. 使用注册表编辑器验证**DSA 先前还原计数**中的值是否等于前一个值加1。 如果这不是正确的值, 并且在事件查看器中找不到事件 ID 1109 的条目, 请确认域控制器的 service pack 是最新的。 你不能在同一 VHD 上再次尝试此过程。 你可以在 VHD 副本上重试, 或通过从步骤1开始, 在正常模式下重试。
13. 打开注册表编辑器。

## <a name="usn-and-usn-rollback"></a>USN 和 USN 回滚

本部分介绍由于使用较旧版本的虚拟机还原 Active Directory 数据库时可能会发生的复制问题。 有关 Active Directory 复制过程的更多详细信息, 请参阅[Active Directory 复制概念](../replication/active-directory-replication-concepts.md)

## <a name="usns"></a>Usn

Active Directory 域服务 (AD DS) 使用更新序列号 (Usn) 来跟踪域控制器之间的数据复制。 每次对目录中的数据进行更改时, USN 会递增以指示已进行了更改。

对于目标域控制器存储的每个目录分区, Usn 用于跟踪域控制器引入其数据库的最新源更新, 以及存储副本的每个其他域控制器的状态目录分区。 当域控制器将更改复制到其他域控制器时, 它们会查询其复制伙伴的 Usn 更改, 这些更改大于域控制器从每个伙伴收到的最后一次更改的 USN。

以下两个复制元数据表包含 Usn。 源域控制器和目标域控制器使用它们来筛选目标域控制器需要的更新。

1. **最新矢量**:目标域控制器为跟踪从所有源域控制器接收的源更新而维护的表。 当目标域控制器请求对目录分区进行更改时, 它将为源域控制器提供最新的矢量。 然后, 源域控制器使用此值来筛选它发送到目标域控制器的更新。 在成功完成复制循环后, 源域控制器会将其最新矢量发送到目标, 以确保目标域控制器知道它已与每个域控制器同步原始更新和更新的级别与源相同。  
2. **高水印**:一个值, 目的域控制器维护此值以跟踪从特定分区的特定源域控制器接收的最新更改。 高水位线阻止源域控制器发送目标域控制器已收到的更改。  

## <a name="directory-database-identity"></a>目录数据库标识

除了 Usn 以外, 域控制器还跟踪源复制伙伴的目录数据库。 服务器上运行的目录数据库的标识与服务器对象本身的标识分开维护。 每个域控制器上的目录数据库标识存储在 "NTDS 设置" 对象的**invocationID**属性中, 该属性位于以下轻型目录访问协议 (LDAP) 路径下: CN = NTDS Settings, Cn = ServerName, cn= Servers, cn =*SiteName*, Cn = Sites, Cn = Configuration, Dc =*ForestRootDomain*。 服务器对象标识存储在 NTDS 设置对象的**objectGUID**特性中。 服务器对象的标识决不会更改。 但是, 当服务器上出现系统状态还原过程或添加应用程序目录分区, 并随后从服务器重新添加时, 目录数据库的标识会更改。 (另一种情况: 当 HyperV 实例在包含虚拟 DC VHD 的分区上触发其 VSS 编写器时, 来宾反过来触发其自己的 VSS 编写器 (与上述备份/还原所使用的机制相同的机制), 这是 invocationID 的另一种方法。&

因此, **invocationID**会有效地将域控制器上的一组原始更新与特定版本的目录数据库相关联。 最新的矢量和高水位标记表分别使用**invocationID**和 DC GUID, 以便域控制器知道复制信息来自 Active Directory 数据库的哪个副本。

**InvocationID**是一个全局唯一标识符 (GUID) 值, 在运行命令**repadmin/showrepl**后, 在输出顶部附近可见。 以下文本表示命令的示例输出:

   ```
   Repadmin: running command /showrepl against full DC local host
   Default-First-Site-Name\VDC1
   DSA Options: IS_GC
   DSA object GUID: 966651f3-a544-461f-9f2c-c30c91d17818
   DSA invocationID: b0d9208b-8eb6-4205-863d-d50801b325a9
   ```

如果在域控制器上正确还原 AD DS, **invocationID**将重置。 作为此更改的结果, 你将会遇到复制流量的增加–这是相对于正在复制的分区大小的持续时间

例如, 假设 VDC1 和 DC2 是同一域中的两个域控制器。 下图显示了当在适当的还原情况下重置 invocationID 值时, 有关 VDC1 的信息。

![](media/virtualized-domain-controller-architecture/Dd363553.ca71fc12-b484-47fb-991c-5a0b7f516366(WS.10).gif)

## <a name="usn-rollback"></a>USN 回滚

在对 usn 的正常更新进行规避并且域控制器尝试使用低于其最新更新的 USN 时, 会发生 USN 回滚。 在大多数情况下, 将检测 USN 回滚, 并将停止复制, 然后再创建林中的分歧。 

USN 回滚的原因有很多, 例如, 当使用旧虚拟硬盘 (VHD) 文件或进行物理到虚拟转换 (P2V 转换) 时, 不确保物理计算机在转换后永久保持脱机状态。 请采取以下预防措施, 以确保不会发生 USN 回滚:

   - 如果未运行 Windows Server 2012 或更高版本, 则不需要使用域控制器虚拟机的快照。
   - 请勿复制域控制器 VHD 文件。  
   - 如果未运行 Windows Server 2012 或更高版本, 请不要导出运行域控制器的虚拟机。  
   - 不要还原域控制器, 或者尝试通过任何其他方式 (如 Windows Server 备份) 来回滚 Active Directory 数据库的内容。  

在某些情况下, 可能会检测不到 USN 回滚。 在其他情况下, 它可能会导致其他复制错误。 在这种情况下, 需要确定问题的范围并及时处理。 有关如何删除因 USN 回滚而可能发生的延迟对象的信息, 请参阅 Microsoft 知识库中的[过时 Active Directory 对象在 Windows Server 2003 中生成事件 ID 1988](https://go.microsoft.com/fwlink/?linkid=137185) 。

## <a name="usn-rollback-detection"></a>USN 回滚检测

大多数情况下, 如果检测到不正确的还原过程 , 则不会重置 USN 回滚。 在不正确的域控制器还原操作之后, Windows Server 2008 对不适当的复制提供保护。 这种保护是由以下事实触发的: 不正确的还原操作导致 Usn 较低, 以表示复制伙伴已经收到的原始更改。

在 Windows Server 2008 和 Windows Server 2003 SP1 中, 当目标域控制器通过使用以前使用的 USN 请求更改时, 目标域控制器会解释其源复制伙伴的响应, 以表示其复制元数据已过时。 这表示源域控制器上的 Active Directory 数据库已回滚到以前的状态。 例如, 虚拟机的 VHD 文件已回滚到以前的版本。 在这种情况下, 目标域控制器会在已被确定为已完成不正确还原的域控制器上启动以下隔离措施:

   - AD DS 暂停 Net Logon 服务, 这会阻止用户帐户和计算机帐户更改帐户密码。 如果在不正确的还原之后发生此类更改, 则此操作将阻止丢失此类更改。  
   - AD DS 禁用入站和出站 Active Directory 复制。  
   - AD DS 将在目录服务事件日志中生成事件 ID 2095 以指示条件。  

下图显示了在虚拟机上运行的 VDC2 (目标域控制器) 上检测到 USN 回滚时所发生的事件的顺序。 在此图中, 当复制伙伴检测到 VDC2 已发送最新的 USN 值时, 将在 VDC2 上进行 USN 回滚的检测, 这表明 Vdc2 数据库已回滚时间不正确。

![](media/virtualized-domain-controller-architecture/Dd363553.373b0504-43fc-40d0-9908-13fdeb7b3f14(WS.10).gif)

如果目录服务事件日志报告事件 ID 2095, 请立即完成以下过程。

## <a name="to-resolve-event-id2095"></a>解析事件 ID 2095

1. 将记录错误的虚拟机与网络隔离开来。
2. 尝试确定是否有任何更改源自此域控制器并传播到其他域控制器。 如果事件是启动虚拟机的快照或副本的结果, 请尝试确定 USN 回滚发生的时间。 然后, 你可以检查该域控制器的复制伙伴, 以确定复制是否发生了。

   您可以使用 Repadmin 工具来做出此决定。 有关如何使用 Repadmin 的信息, 请参阅[使用 Repadmin 进行 Active Directory 复制的监视和故障排除](https://go.microsoft.com/fwlink/?linkid=122830)。 如果无法自行确定此问题, 请联系[Microsoft 支持部门](https://support.microsoft.com)以获得帮助。

3. 强制降级域控制器。 这涉及到清理域控制器的元数据并占用操作主机 (也称为灵活单主机操作或 FSMO) 角色。 有关详细信息, 请参阅 Microsoft 知识库中的[如何在 Windows server 2003、Windows server 2008 和 Windows server 2008 R2 中检测和恢复 usn 回滚中](https://go.microsoft.com/fwlink/?linkid=137182)的 "从 Usn 回滚恢复" 部分。
4. 删除域控制器的所有以前的 VHD 文件。

## <a name="undetected-usn-rollback"></a>未检测到的 USN 回滚

在以下两种情况下, 可能不会检测 USN 回滚:

1. VHD 文件附加到同时在多个位置运行的不同虚拟机。  
2. 还原的域控制器上的 USN 已超过其他域控制器已收到的最后一个 USN。  

在第一种情况下, 其他域控制器可能会随其中一台虚拟机一起复制, 而不会将更改复制到另一台虚拟机上。 林的这种分歧很难检测到, 并会导致不可预知的目录响应。 如果物理计算机和虚拟机都在同一网络上运行, 则可能会在 P2V 迁移后出现这种情况。 如果从同一个物理域控制器创建了多个虚拟域控制器, 然后在同一网络上运行, 也可能会发生这种情况。

在第二种情况下, Usn 范围适用于两个不同的更改集。 此操作可在未检测到的情况下继续运行。 每当修改在此时间创建的对象时, 都会检测到延迟对象并将其报告为事件查看器中的事件 ID 1988。 下图显示了在这种情况下如何检测 USN 回滚。

![](media/virtualized-domain-controller-architecture/Dd363553.63565fe0-d970-4b4e-b5f3-9c76bc77e2d4(WS.10).gif)

## <a name="read-only-domain-controllers"></a>只读域控制器

Rodc 是在 Active Directory 数据库中托管分区的只读副本的域控制器。 Rodc 不会将任何更改复制到其他域控制器。 但是, 如果 RODC 从受 USN 回滚影响的可写域控制器进行复制, 则 RODC 也会受到影响。

不建议使用快照还原 RODC。 使用与 Active Directory 兼容的备份应用程序还原 RODC。 此外, 与可写域控制器一样, 必须谨慎考虑不允许 RODC 脱机, 使其超过 tombstone 生存期。 这种情况可能会导致 RODC 上的延迟对象。

有关 Rodc 的详细信息, 请参阅[只读域控制器规划和部署指南](../../deploy/rodc/read-only-domain-controller-updates.md)。
