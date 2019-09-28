---
title: AD 林恢复-确定如何恢复林
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: d604efded5b6a2ff3911a92f52817498f43c9933
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369174"
---
# <a name="determine-how-to-recover-the-forest"></a>确定如何恢复林

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

恢复整个 Active Directory 林涉及到在林中的每个域控制器（DC）上将其还原为备份或重新安装 Active Directory 域服务（AD DS）。 恢复林会将林中的每个域还原到上次受信任的备份时的状态。 因此，还原操作将导致至少丢失以下 Active Directory 数据：

- 上次受信任的备份后添加的所有对象（例如用户和计算机）
- 自上次受信任的备份以来对现有对象所做的所有更新
- 自上次信任的备份以来，对 AD DS 中的配置分区或架构分区所做的所有更改（例如架构更改）

对于林中的每个域，必须知道域管理员帐户的密码。 最好是内置管理员帐户的密码。 还必须知道 DSRM 密码才能执行 DC 的系统状态还原。 通常情况下，最好在安全位置将管理员帐户和 DSRM 密码历史记录存档，只要备份有效，即在 tombstone 生存期内或在删除的对象生存期内（如果 Active Directory 回收已启用 Bin。 你还可以将 DSRM 密码与域用户帐户同步，以便更容易记住。 有关详细信息，请参阅知识库文章[961320](https://support.microsoft.com/kb/961320)。 在准备过程中，必须在林恢复之前对 DSRM 帐户进行同步。

> [!NOTE]
> 默认情况下，管理员帐户是内置 Administrators 组的成员，域管理员组和企业管理员组也是如此。 此组对域中的所有 Dc 都具有完全控制权。

## <a name="determining-which-backups-to-use"></a>确定要使用的备份

定期备份每个域的至少两个可写 Dc，以便您可以选择多个备份。 请注意，不能使用只读域控制器（RODC）的备份来还原可写 DC。 建议你通过使用在发生故障之前经过几天的备份来还原 Dc。 通常，必须确定 recentness 与还原的数据的 safeness 之间的平衡点。 选择最新的备份会恢复更有用的数据，但可能会增加将危险数据重新引入到还原的林中的风险。

还原系统状态备份取决于备份的原始操作系统和服务器。 例如，不应将系统状态备份还原到其他服务器。 在这种情况下，你可能会看到以下警告：

"指定的备份所在的服务器不同于当前的备份。 不建议使用备份到备用服务器执行系统状态恢复，因为该服务器可能会变得不可用。 是否确实要使用此备份恢复当前服务器？ "

如果需要将 Active Directory 还原到不同的硬件，请创建完整的服务器备份，并计划执行完整服务器恢复。

> [!IMPORTANT]
> 从 Windows Server 2008 开始，不支持在新硬件或相同硬件上将系统状态备份还原到新安装的 Windows Server。 如果在同一硬件上重新安装了 Windows Server，如本指南后面的建议，则可以按以下顺序还原域控制器：
>
> 1. 执行完整服务器还原，以便还原操作系统以及所有文件和应用程序。
> 2. 使用 wbadmin 执行系统状态还原，以将 SYSVOL 标记为权威。
>
> 有关详细信息，请参阅 Microsoft 知识库文章[249694](https://support.microsoft.com/kb/249694)。

如果出现故障的时间未知，请进一步进行调查，确定保存林的最后一个安全状态的备份。 这种方法不太理想。 因此，我们强烈建议您每日保存有关 AD DS 的运行状况状态的详细日志，以便在林范围内发生故障时，可以确定故障的大致时间。 还应保留备份的本地副本以实现更快的恢复。

如果启用了 Active Directory 回收站，则备份生存期等于**deletedObjectLifetime**值或**tombstoneLifetime**值（以较小者为准）。 有关详细信息，请参阅[Active Directory 回收站循序渐进指南](https://go.microsoft.com/fwlink/?LinkId=178657)（@no__t 为-1。

作为替代方法，你还可以使用 Active Directory 数据库装载工具（Dsamain.exe）和轻型目录访问协议（LDAP）工具（如 Ldp.exe 或 Active Directory 用户和计算机）来识别哪个备份具有最新的安全状态树林. Windows Server 2008 和更高版本的 Windows Server 操作系统中包含的 Active Directory 数据库装载工具公开作为 LDAP 服务器存储在备份或快照中的 Active Directory 数据。 然后，可以使用 LDAP 工具来浏览数据。 此方法的优点是不需要重新启动目录服务还原模式（DSRM）中的任何 DC 来检查 AD DS 备份的内容。

有关使用 Active Directory 数据库装载工具的详细信息，请参阅[Active Directory 数据库装载工具循序渐进指南](https://technet.microsoft.com/library/cc753609\(WS.10\).aspx)。

你还可以使用**ntdsutil snapshot**命令创建 Active Directory 数据库的快照。 通过计划定期创建快照的任务，您可以在一段时间内获取 Active Directory 数据库的其他副本。 你可以使用这些副本来更好地识别林范围的故障发生的时间，然后选择要还原的最佳备份。 若要创建快照，请使用 Windows Server 2008 附带的**ntdsutil**版本或适用于 windows Vista 或更高版本的远程服务器管理工具（RSAT）。 目标 DC 可以运行任何版本的 Windows Server。 有关使用**ntdsutil snapshot**命令的详细信息，请参阅[snapshot](https://technet.microsoft.com/library/cc731620\(WS.10\).aspx)。

## <a name="determining-which-domain-controllers-to-restore"></a>确定要还原的域控制器

在决定要还原哪个域控制器时，简化还原过程很重要。 建议为每个域使用一个专用 DC，作为还原的首选 DC。 专用还原 DC 使你可以更轻松地计划和执行林恢复，因为你使用的是用于执行还原测试的相同源配置。 你可以编写恢复脚本，而不会争用不同的配置，例如 DC 是否持有操作主机角色，或者它是 GC 还是 DNS 服务器。

> [!NOTE]
> 尽管不建议在简单的情况下还原操作主机角色持有者，但某些组织可能会选择还原一个以获得其他好处。 例如，还原 RID 主机可能有助于防止在恢复过程中管理 Rid 的问题。  

选择最符合以下条件的 DC：

- 可写的 DC。 这是必需的。

- 在支持 VM 生成 id 的虚拟机监控程序上运行 Windows Server 2012 作为虚拟机的 DC。 此 DC 可用作克隆的源。
- 可以在物理上或虚拟网络上访问的 DC，并且最好位于数据中心内。 这样，便可以在林恢复过程中轻松地将它与网络隔离开来。
- 具有良好的完整服务器备份的 DC。 一个好的备份是可以成功还原的备份，在发生故障之前的几天内执行，并包含尽可能多的有用数据。
- 在发生故障之前作为域名系统（DNS）服务器的 DC。 这节省了重新安装 DNS 所需的时间。
- 如果还使用 Windows 部署服务，请选择未配置为使用 BitLocker 网络解锁的 DC。 在这种情况下，不支持将 BitLocker 网络解锁用于在林恢复期间从备份还原的第一个 DC。

   BitLocker 网络解锁，因为在已部署 Windows 部署服务（WDS *）的 dc*上*不能*使用 BitLocker 网络解锁，因为这样做会导致第一个 DC 需要 Active Directory 和 WDS 才能工作，以便解锁. 但在还原第一个 DC 之前，Active Directory 尚不能用于 WDS，因此无法解锁。

   若要确定是否已将 DC 配置为使用 BitLocker 网络解锁，请检查以下注册表项中是否标识了网络解锁证书：

   HKEY_LOCAL_MACHINESoftwarePoliciesMicrosoftSystemCertificatesFVE_NKP

维护处理或还原包含 Active Directory 的备份文件时的安全过程。 林恢复的紧急性会无意中导致忽视的安全最佳做法。 有关详细信息，请参阅 [Best 实践指南中标题为 "建立域控制器备份和还原策略" 的部分，用于保护 Active Directory 安装和日常操作：第 II 部分 @ no__t。

## <a name="identify-the-current-forest-structure-and-dc-functions"></a>标识当前林结构和 DC 函数

通过标识林中的所有域确定当前林结构。 创建每个域中的所有 Dc 的列表，尤其是包含备份的 Dc 和可作为克隆源的虚拟化 Dc。 目录林根级域的 Dc 列表最为重要，因为你将首先恢复该域。 还原目录林根级域后，可以使用 Active Directory 管理单元获取林中的其他域、Dc 和站点的列表。

准备一个表，其中显示域中每个 DC 的功能，如以下示例中所示。 这将帮助你在恢复后恢复到林的预故障配置。

|DC 名称|操作系统|FSMO|GC|RODC|备份|DNS|服务器核心|VM|VM-GenID|  
|-------------|----------------------|----------|--------|----------|------------|---------|-----------------|--------|---------------|  
|DC_1|Windows Server 2012|架构主机，域命名主机|是|否|是|否|否|是|是|  
|DC_2|Windows Server 2012|无|是|否|是|是|否|是|是|  
|DC_3|Windows Server 2012|结构主机|否|否|否|是|是|是|是|  
|DC_4|Windows Server 2012|PDC 模拟器，RID 主机|是|否|否|否|否|是|否|  
|DC_5|Windows Server 2012|无|否|否|是|是|否|是|是|  
|RODC_1|Windows Server 2008 R2|无|是|是|是|是|是|是|否|  
|RODC_2|Windows Server 2008|无|是|是|否|是|是|是|否|  

对于林中的每个域，标识包含该域的 Active Directory 数据库的受信任备份的单个可写 DC。 选择用于还原 DC 的备份时，请务必小心。 如果失败的日期和原因大约是已知的，则一般建议使用在该日期之前数天内进行的备份。
  
在此示例中，有四个备份候选项：DC_1、DC_2、DC_4 和 DC_5。 在这些备份候选项中，只还原一个。 建议 DC 的原因如下：  

- 它满足使用它作为虚拟化 DC 克隆的源的要求，也就是说，它在支持 VM 生成 id 的虚拟机监控程序上运行 Windows Server 2012 作为虚拟 DC，并运行允许克隆的软件（或者，如果无法克隆，则可以将其删除d）。 还原后，PDC 模拟器角色将被强制转移到该服务器，并且可以添加到域的可克隆域控制器组。  
- 它运行完整安装的 Windows Server 2012。 运行服务器核心安装的 DC 不太方便作为恢复目标。  
- 它是 DNS 服务器。 因此，无需重新安装 DNS。  

> [!NOTE]
> 由于 DC_5 不是全局目录服务器，因此它还有一个优点，那就是不需要在还原后删除全局编录。 但是，即使 DC 也是全局编录服务器，因为从 Windows Server 2012 开始，默认情况下所有 Dc 都是全局编录服务器，并且建议在还原后删除并添加全局编录。任何情况下的恢复过程。  

## <a name="recover-the-forest-in-isolation"></a>隔离恢复林

首选方案是在第一个还原的 DC 恢复到生产环境之前关闭所有可写 Dc。 这可确保所有危险数据不会复制回恢复的林中。 关闭所有操作主机角色持有者尤其重要。  

> [!NOTE]
> 在某些情况下，你可能会将计划为每个域恢复的第一个 DC 移到隔离的网络，同时允许其他 Dc 保持联机，以最大程度地减少系统停机时间。 例如，如果要从失败的架构升级中恢复，则可以选择在执行隔离时在生产网络上运行域控制器。

如果正在运行虚拟化 Dc，则可以将它们移到与将执行恢复的生产网络隔离的虚拟网络中。 将虚拟化 Dc 移动到单独的网络有两个好处：  

- 由于隔离的 Dc 被隔离，因此不能再次发生导致林恢复的问题。  
- 可以在单独的网络上执行虚拟化 DC 克隆，以便在将关键的 Dc 恢复到生产网络之前，可以运行和测试这些 Dc。

如果你在物理硬件上运行 Dc，请断开你计划在目录林根级域中还原的第一个 DC 的网络电缆。 如果可能，还应断开所有其他 Dc 的网络电缆。 这会阻止 Dc 进行复制（如果在林恢复过程中意外启动）。  

在分布在多个位置的大型林中，可能很难确保所有可写 Dc 都已关闭。 出于此原因，恢复步骤（如重置计算机帐户和 krbtgt 帐户，以及清除元数据）旨在确保恢复的可写 Dc 不会与危险的可写 Dc 一起复制（以防某些情况仍处于联机状态林）。  
  
不过，只有在离线使用可写 Dc 后，才能保证不会进行复制。 因此，应尽可能部署远程管理技术，以帮助你在林恢复期间关闭并物理隔离可写 Dc。  
  
可写 Dc 处于脱机状态时，Rodc 可以继续运行。 其他 DC 不会直接复制任何 RODC 中的任何更改，尤其是无架构或配置容器更改-因此，在恢复过程中，这些更改不会带来与可写 Dc 相同的风险。 在所有可写 Dc 恢复并联机后，应重新生成所有 Rodc。  
  
当恢复操作并行进行时，Rodc 将继续允许访问其各自站点中缓存的本地资源。 未缓存在 RODC 上的本地资源会将身份验证请求转发给可写 DC。 这些请求将失败，因为可写 Dc 处于脱机状态。 某些操作（例如密码更改）在恢复可写 Dc 之前也不起作用。  
  
如果你使用的是中心辐射型网络体系结构，则可以先关注如何恢复中心站点中的可写 Dc。 稍后，你可以在远程站点中重建 Rodc。  
  
## <a name="next-steps"></a>后续步骤

- [AD 林恢复 - 先决条件](AD-Forest-Recovery-Prerequisties.md)  
- [AD 林恢复-设计自定义林恢复计划](AD-Forest-Recovery-Devising-a-Plan.md)  
- [AD 林恢复-识别问题](AD-Forest-Recovery-Identify-the-Problem.md)
- [AD 林恢复-确定如何恢复](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [AD 林恢复-执行初始恢复](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [AD 林恢复 - 过程](AD-Forest-Recovery-Procedures.md)  
- [AD 林恢复-常见问题](AD-Forest-Recovery-FAQ.md)  
- [AD 林恢复-恢复多域林中的单个域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [AD 林恢复-通过 Windows Server 2003 域控制器恢复林](AD-Forest-Recovery-Windows-Server-2003.md)  
