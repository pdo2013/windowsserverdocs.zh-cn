---
title: AD 林恢复-确定如何恢复林
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 30d23d977b4d7cd320d78ff340120df7013dee4c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817728"
---
# <a name="determine-how-to-recover-the-forest"></a>确定如何恢复林

>适用于：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

恢复整个 Active Directory 林中涉及从备份中还原或重新安装 Active Directory 域服务 (AD DS) 林中每个域控制器 (DC) 上。 恢复林将还原的林中每个域到其状态时的最后一个受信任的备份。 因此，还原操作将导致丢失至少以下 Active Directory 数据：

- 在受信任的上次备份后添加的所有对象 （如用户和计算机）
- 对所做的现有对象由于受信任的最后一个备份的所有更新
- 自最后一个受信任的备份以来所做的配置分区或架构分区 （例如架构更改） 的 AD DS 中的所有更改

对于每个域林中，域管理员帐户的密码必须已知。 最好是，这是内置的管理员帐户的密码。 此外必须知道要执行系统状态还原的 DC 的 DSRM 密码。 一般情况下，它是最好的只要备份是有效的也就是说，或已删除的对象生存期内逻辑删除生存期期间如果 Active Directory 回收存档的管理员帐户和 DSRM 密码历史记录在安全的位置启用 bin。 你还可以使用域用户帐户同步 DSRM 密码，以使其更易于记忆。 有关详细信息，请参阅知识库文章[961320](https://support.microsoft.com/kb/961320)。 林恢复，准备过程之前，必须完成同步 DSRM 帐户。

> [!NOTE]
> 管理员帐户都是 Domain Admins 和 Enterprise Admins 组是默认情况下，内置管理员组的成员。 此组具有在域中的所有域控制器的完全控制。

## <a name="determining-which-backups-to-use"></a>确定要使用的备份

为每个域至少两个可写 Dc 定期备份使您有多个备份，可供选择。 请注意，不能使用只读域控制器 (RODC) 的备份还原可写 DC。 我们建议您通过使用执行之前失败的匹配项的几天的备份还原域控制器。 一般情况下，您必须确定趋今度和还原的数据 safeness 之间的权衡。 选择将较新的备份恢复更为有用的数据，但它可能会增加重新危险数据引入到已还原林的风险。

还原系统状态备份依赖于原始操作系统和服务器的备份。 例如，不应还原系统状态备份到另一台服务器。 在这种情况下，可能会看到以下警告：

"指定的备份是服务器的与当前的一个不同。 我们不建议执行的备份系统状态恢复到备用服务器，因为服务器可能会变得不可用。 是否确实想要用于此备份恢复当前服务器？"

如果你需要将 Active Directory 还原到不同的硬件，创建完整服务器备份和计划，以执行整个服务器恢复。

> [!IMPORTANT]
> 从 Windows Server 2008 开始，它不是支持还原系统状态备份到新硬件或在同一硬件上新安装的 Windows Server。 如果在同一硬件上重新安装 Windows 服务器按照本指南后面的建议，则可以还原此顺序中的域控制器：
>
> 1. 若要还原操作系统和所有文件和应用程序中执行整个服务器都还原。
> 2. 执行系统状态还原使用 wbadmin.exe，以将 SYSVOL 标记为权威。
>
> 有关详细信息，请参阅 Microsoft 知识库文章[249694](https://support.microsoft.com/kb/249694)。

如果失败的匹配项的时间未知，则进一步调查，识别保留在林中的最后一个安全状态的备份。 此方法不太可取。 因此，我们强烈建议保留有关 AD DS 的运行状况状态的详细的日志在每日基础上，以便如果全林性失败，可以标识失败的大致时间。 此外应保留备份，以启用更快地恢复的本地副本。

如果启用了 Active Directory 回收站，备份的生存期是否等于**deletedObjectLifetime**值或**tombstoneLifetime**值小者为准。 有关详细信息，请参阅[Active Directory 回收站分步指南](https://go.microsoft.com/fwlink/?LinkId=178657)(https://go.microsoft.com/fwlink/?LinkId=178657)。

或者，还可以使用 Active Directory 数据库装载工具 (Dsamain.exe) 和轻型目录访问协议 (LDAP) 工具，如 Ldp.exe 或 Active Directory 用户和计算机，以确定哪个备份具有的最后一个安全状态林。 包含在 Windows Server 2008 和更高版本的 Windows Server 操作系统，Active Directory 数据库装载工具将 Active Directory 数据存储在备份或快照中作为 LDAP 服务器公开。 然后，您可以使用 LDAP 工具浏览数据。 这种方法的优点是不需要您重新启动任何 DC 在目录服务还原模式 (DSRM) 来检查 AD DS 备份的内容。

有关使用 Active Directory 数据库装载工具的详细信息，请参阅[Active Directory 数据库装载工具循序渐进指南](https://technet.microsoft.com/library/cc753609\(WS.10\).aspx)。

此外可以使用**ntdsutil 快照**命令以创建 Active Directory 数据库的快照。 通过计划任务定期创建快照，可以随着时间的推移获取 Active Directory 数据库的其他副本。 可以使用这些副本以更好地识别发生全林性失败时，然后选择要还原的最佳备份。 若要创建快照，使用的版本**ntdsutil**随 Windows Server 2008 或远程服务器管理工具 (RSAT) 适用于 Windows Vista 或更高版本。 目标 DC 可以运行任何版本的 Windows Server。 有关使用详细信息**ntdsutil 快照**命令，请参阅[快照](https://technet.microsoft.com/library/cc731620\(WS.10\).aspx)。

## <a name="determining-which-domain-controllers-to-restore"></a>确定要还原的域控制器

决定要还原的域控制器时，还原过程的易用性是一个重要因素。 建议已还原的首选 dc 每个域的专用的 DC。 专用的还原 DC 轻松可靠地计划和执行林恢复，因为使用相同的源配置用于执行还原测试。 可以编写脚本恢复，并不会争用具有不同配置，例如是否 DC 持有操作主机角色，或者它是 GC 或 DNS 服务器，还是不。

> [!NOTE]
> 尽管不建议还原操作主机角色所有者为了简单起见，某些组织可能选择还原另一个用于其他优点。 例如还原 RID 主机可能有助于防止在恢复期间管理 Rid 的问题。  

选择最能满足以下条件的 DC:

- 可写 DC。 这是必需的。

- 在支持 VM 生成 Id 的虚拟机监控程序上作为虚拟机运行 Windows Server 2012 的 DC。 此 DC 可以克隆使用作为源。
- 可访问，以物理方式或在虚拟网络中，并且最好位于数据中心中的 DC。 这样一来，您可以轻松地将其隔离从网络林恢复过程。
- 具有良好的完整服务器备份 DC。 一个好的备份是可以成功还原，拍摄在发生故障之前的几天，并包含作为更有用的数据的备份。
- 已在发生故障之前的域名系统 (DNS) 服务器 DC。 这样，若要重新安装 DNS 所需的时间。
- 如果还使用 Windows 部署服务，请选择未配置为使用 BitLocker 网络解锁的 DC。 在这种情况下，BitLocker 网络解锁不支持用于从备份还原林恢复过程的第一个 DC。

   为 BitLocker 网络解锁*仅*密钥保护程序*不能*使用 Dc 上的已部署 Windows 部署服务 (WDS)，因为这样做会导致一个方案要求的第一个 DC 的位置Active Directory 和 WDS 才能解锁工作。 但在还原第一个域控制器之前，Active Directory 尚不可用的 WDS，因此它不能解锁。

   若要确定 DC 配置为使用 BitLocker 网络解锁，检查网络解锁证书已在以下注册表项中标识：

   HKEY_LOCAL_MACHINESoftwarePoliciesMicrosoftSystemCertificatesFVE_NKP

维护安全过程处理或还原包括 Active Directory 的备份文件时。 紧急性附带林恢复可能会无意中导致忽视安全最佳方案。 有关详细信息，请参阅中标题为"建立域控制器备份和还原策略"部分[Active Directory 安全安装和日常操作的最佳做法指南：第二部分](https://technet.microsoft.com/library/bb727066.aspx)。

## <a name="identify-the-current-forest-structure-and-dc-functions"></a>确定当前林结构和 DC 函数

确定当前的林结构通过标识林中所有域。 请在每个域，特别是域控制器都有备份和虚拟化的 Dc 可以是用于克隆的源中的所有 Dc 的列表。 为目录林根域的 Dc 列表将最重要，因为你将首先恢复此域。 还原目录林根域之后，可以通过使用 Active Directory 管理单元中获取其他域、 域控制器，并在林中的站点的列表。

准备在域中，显示每个 DC 的函数的表，如下面的示例中所示。 这将帮助您还原回的林恢复后的前失败配置。

|DC 名称|操作系统|FSMO|GC|RODC|备份|DNS|服务器核心|VM|VM-GenID|  
|-------------|----------------------|----------|--------|----------|------------|---------|-----------------|--------|---------------|  
|DC_1|Windows Server 2012|架构主机域命名主机|是|否|是|否|否|是|是|  
|DC_2|Windows Server 2012|无|是|否|是|是|否|是|是|  
|DC_3|Windows Server 2012|结构主机|否|否|否|是|是|是|是|  
|DC_4|Windows Server 2012|PDC 仿真器、 RID 主机|是|否|否|否|否|是|否|  
|DC_5|Windows Server 2012|无|否|否|是|是|否|是|是|  
|RODC_1|Windows Server 2008 R2|无|是|是|是|是|是|是|否|  
|RODC_2|Windows Server 2008|无|是|是|否|是|是|是|否|  

对于每个林中的域，标识的单个可写 DC，具有受信任的域的 Active Directory 数据库的备份。 选择要还原 DC 的备份时要格外小心。 如果在大约已知的日期和失败的原因，常规建议是使用已在该日期之前的几天的备份。
  
在此示例中，有四个备份的候选项：DC_1、 DC_2、 DC_4 和 DC_5。 这些备份的候选项，则还原只有一个。 建议的 DC 是 DC_5 原因如下：  

- 它满足是使用它作为虚拟化 DC 克隆的源的要求，它在支持 VM 生成 Id 的虚拟机监控程序上的虚拟 DC 运行时软件可以是运行 Windows Server 2012 克隆 （或如果不能进行克隆，可以移除的d) 中。 还原后，PDC 仿真器角色将开始强制转移到服务器，该报表可以添加到域的组中源域控制器。  
- 它在运行 Windows Server 2012 的完整安装。 运行服务器核心安装的 DC 可以不是很方便为目标进行恢复。  
- 它是 DNS 服务器。 因此，DNS 无需重新安装。  

> [!NOTE]
> 由于 DC_5 不是全局编录服务器，它还具有一个优点在于，不需要还原后删除全局编录。 但 DC 也是全局编录服务器不是一个决定性因素，因为从 Windows Server 2012 开始，所有域控制器是全局编录服务器，默认情况下，和删除和后还原建议作为林的一部分，添加全局编录恢复在任何情况下处理。  

## <a name="recover-the-forest-in-isolation"></a>恢复中隔离的林

首选的方案是关闭之前的第一个还原 DC 的所有可写域控制器将恢复到生产环境。 这可确保任何危险数据不会复制回已恢复林。 它是关闭的情况下所有操作主机角色持有者尤其重要。  

> [!NOTE]
> 可能情况下移动你计划同时允许其他域控制器来保持联机，以便最大程度减少系统停机时间恢复为每个域到隔离的网络的第一个 DC 的位置。 例如，如果要从失败的架构的升级中恢复，你可以选择要保留在隔离执行恢复步骤时，生产网络上运行的域控制器。

如果运行虚拟化的 Dc 时，您可以将它们移到与生产网络隔离的虚拟网络将执行恢复的位置。 将虚拟化的 Dc 移到单独的网络提供了两个好处：  

- 已恢复域控制器不能从导致林恢复，因为它们是独立的问题的再次发生。  
- 虚拟化 DC 克隆可以执行单独的网络上，以便可以运行大量关键的域控制器和测试之前重新转到生产网络。

如果在物理硬件上运行的 Dc，断开连接您计划还原目录林根级域中的第一个 DC 的网络电缆。 如果可能，还断开连接所有其他域控制器的网络的电缆。 这可以防止 Dc 复制，如果它们不小心启动林恢复过程。  

在跨多个位置分散的大型林中，它可能很难保证所有可写域控制器已关闭。 出于此原因，恢复步骤 — 如重置计算机帐户和 krbtgt 帐户，除了为元数据清除 — 设计为了确保已恢复的可写 Dc 不会有危险的可写 Dc 将复制 (如果有些中仍处于联机状态林）。  
  
但是，仅通过使可写 Dc 脱机可以可以确保不会进行复制。 因此，只要有可能，应部署可以帮助您关闭的情况下，并在林恢复过程以物理方式隔离可写 Dc 的远程管理技术。  
  
Rodc 可以继续运行时可写 Dc 处于脱机状态。 其他 DC 不会直接将任何更改复制从任何 RODC — 尤其是，任何架构更改或配置容器，因此它们确实带来在恢复期间的可写 Dc 的危险。 所有可写域控制器已恢复并处于联机状态后，应重新生成所有 Rodc。  
  
Rodc 将继续允许同时并行在进行恢复操作在其相应站点中缓存的本地资源的访问权限。 不会缓存在 RODC 的本地资源将具有身份验证请求转发到可写 DC。 这些请求将失败，因为可写 Dc 处于脱机状态。 直到您恢复可写 Dc，如密码更改某些操作也将无法工作。  
  
如果使用中心辐射型网络体系结构，您可以先专注于恢复中心站点中的可写域控制器。 更高版本，可以重新生成远程站点中的 Rodc。  
  
## <a name="next-steps"></a>后续步骤

- [AD 林恢复的系统必备组件](AD-Forest-Recovery-Prerequisties.md)  
- [AD 林恢复-设计出一个自定义林恢复计划](AD-Forest-Recovery-Devising-a-Plan.md)  
- [AD 林恢复-识别问题](AD-Forest-Recovery-Identify-the-Problem.md)
- [AD 林恢复-确定如何恢复](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [AD 林恢复-执行初始恢复](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [AD 林恢复的过程](AD-Forest-Recovery-Procedures.md)  
- [AD 林恢复-方面的常见问题](AD-Forest-Recovery-FAQ.md)  
- [AD 林恢复-恢复单个的多域林中域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [AD 林恢复-与 Windows Server 2003 域控制器的林恢复](AD-Forest-Recovery-Windows-Server-2003.md)  
