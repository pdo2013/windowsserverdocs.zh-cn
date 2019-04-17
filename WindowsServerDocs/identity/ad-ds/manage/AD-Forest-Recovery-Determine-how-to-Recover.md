---
title: "广告森林恢复-确定如何恢复森林"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: cc2525068225644b0964a2726bd9c0dcf2e0cba0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="determine-how-to-recover-the-forest"></a>确定如何恢复森林  

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

 恢复整个的 Active Directory 森林涉及从备份中还原或在每个森林中的域控制器 (DC) 上重新安装 Active Directory 域服务 (广告 DS)。 恢复森林将森林中的每个域到状态恢复时的最后一个受信任的备份。 因此，恢复操作将导致胡子至少以下 Active Directory 数据：  
  
-   （例如，用户和计算机）所有上次受信任的备份后添加的对象  
  
-   已以来对现有对象受信任的最后一次备份的所有更新  
  
-   所有自上次受信任备份到配置分区或架构分区广告 DS（如方案更改）中的所做的更改  
  
 森林中的每个域，必须已知域管理员帐户的密码。 最好是，这是内置这无法禁用的管理员帐户的密码。 您还必须知道 DSRM 密码才能执行的 DC 状态系统还原。 一般情况下，最好存档管理员帐户并在安全的地方的 DSRM 密码历史记录备份是有效，即，或已删除的对象如果启用了 Active Directory 回收站的生命周期期间 tombstone 生存期期限内，只要。 你还可以以便更加轻松地记住的域的用户帐户同步 DSRM 密码。 有关详细信息，请参阅知识库文章[961320](https://support.microsoft.com/kb/961320)。 同步 DSRM 帐户必须完成提前森林恢复，作为准备工作的一部分。  
  
> [!NOTE]
>  管理员帐户是组成员的内置管理员默认情况下，当是管理员域和企业管理员组。 此组具有域中的所有 Dc 完全控制。  
  
## <a name="determining-which-backups-to-use"></a>如何确定要使用的备份  
 每个域至少两个可写 Dc 定期备份以便拥有多个可供选择的备份。 请注意，不能使用仅阅读域控制器 (RODC) 备份还原可写直流。 我们建议你通过使用拍摄发生故障的之前的几天的备份还原域控制器。 一般情况下，你必须确定 recentness 和还原数据的 safeness 权衡。 选择更多最新的数据备份恢复更有用的数据，但它会增加到还原森林重新引入危险数据的风险。  
  
 还原备份系统状态取决于的原始操作系统和备份的服务器。 例如，你应该不的系统状态备份还原到不同的服务器。 在此情况下，你可能会看到以下警告：  
  
 "的当前不同是服务器的指定的备份。 我们不建议执行系统的状态恢复备份到备用服务器，因为服务器可能会变为不可用。 是否确实要用于恢复当前服务器此备份？"  
  
 如果你需要将 Active Directory 恢复到不同的硬件，创建完整服务器备份，并计划执行完整 server 恢复。  
  
> [!IMPORTANT]
>  开始与 Windows Server 2008，它不支持系统状态备份还原到新硬件或在同一硬件上 Windows Server 的全新安装。 如果稍后本指南中的建议，Windows Server 都在同一硬件上重新安装，你可以还原以此顺序域控制器：  
>   
>  1.  为了还原操作系统和所有文件和应用程序中执行完整服务器都还原。  
> 2.  执行标记为权威 SYSVOL 才能使用 wbadmin.exe 系统状态恢复。  
>   
>  有关详细信息，请参阅 Microsoft 知识库文章[249694](https://support.microsoft.com/kb/249694)。  
  
 如果出现故障的时间不知道，进一步调查来识别按森林的最后一个安全状态的备份。 不是这种方法。 因此，我们强烈建议你保留每天有关广告 DS 运行状况的详细的日志以便可以标识如果树林失败，失败的大致时间。 你还应保留备份，以实现更快恢复本地副本。  
  
 如果启用了 Active Directory 回收站，备份整个使用期限内等于**deletedObjectLifetime**值或**tombstoneLifetime**为准较少的值。 有关详细信息，请参阅[Active Directory 回收站 Step-by-Step 指南](https://go.microsoft.com/fwlink/?LinkId=178657)(https://go.microsoft.com/fwlink/?LinkId=178657)。  
  
 或者，你还可以使用 Active Directory 数据库装载工具 (Dsamain.exe) 还有轻型目录访问协议 (LDAP) 工具，例如 Ldp.exe 或 Active Directory 用户和计算机，确定哪个备份森林的最后一个安全状态。 包含在 Windows Server 2008 和更高版本的 Windows Server 操作系统，该 Active Directory 数据库装载工具公开作为 LDAP 服务器存储在备份或快照的 Active Directory 数据。 然后，你可以使用 LDAP 工具浏览数据。 这种方法优点不需要你重启任何直流中目录服务还原模式 (DSRM) 以检查广告 DS 备份的内容。  
  
 有关使用 Active Directory 数据库装载工具的详细信息，请参阅[Active Directory 数据库装载工具 Step-by-Step 指南](https://technet.microsoft.com/library/cc753609\(WS.10\).aspx)。  
  
 你还可以使用**ntdsutil 快照**命令创建 Active Directory 数据库的快照。 计划任务定期创建的快照，你就可以获得更多份 Active Directory 数据库，随着时间的推移。 你可以使用这些副本更好地识别树林故障发生时，然后选择要还原的最佳备份。 若要创建快照，使用版本**ntdsutil**附带 Windows Server 2008 或远程服务器管理工具 (RSAT) 适用于 Windows Vista 或更高版本。 目标直流可以运行任何版本的 Windows Server。 有关如何使用**ntdsutil 快照**命令，请参阅[快照](https://technet.microsoft.com/library/cc731620\(WS.10\).aspx)。  
  
  
## <a name="determining-which-domain-controllers-to-restore"></a>确定哪些域控制器还原  
 恢复过程轻松时决定要还原的域控制器，将重要的考虑因素。 具有有关每个域，则为还原首选的 DC 专用的 DC 建议。 DC 简化可靠地计划，并执行森林恢复，因为你使用同一来源配置用于执行的专用的恢复还原测试。 你可以脚本恢复，并不争用通过不同的配置，例如是否 DC 包含操作主机角色是否，或无论它 GC 或 DNS 服务器。  
  
> [!NOTE]
>  尽管建议不要还原为简单起见流操作主机角色持有者，也可以选择某些组织还原在另一个用于其他好处。 例如还原 RID 主机可帮助防止恢复期间管理 Rid 的问题。  
  
 选择 DC 最佳满足以下条件：  
  
-   是可写直流。 这是必需的。  
  
-   运行 Windows Server 2012 上虚拟机监控程序支持 VM-GenerationID 虚拟机 DC。 此 DC 可用于为源复制。  
  
-   DC 物理或在虚拟网络，可以被访问，并且最好位于数据中心中。 这样一来，你可以轻松隔离它从网络森林恢复过程。  
  
-   具有完整服务器的正常备份直流。 好备份是可以成功还原，拍摄失败之前, 的几天，并为更有用的数据，以尽可能包含。  
  
-   这是发生故障之前域名系统 (DNS) 服务器 DC。 这样既可以省 DNS 重新安装所需的时间。  
  
-   如果你还可以使用 Windows 部署服务、选择未配置为使用 BitLocker 网络解锁 DC。 在此情况下，BitLocker 网络解锁不支持从备份还原森林恢复过程中的第一个 DC 要用于。  
  
     BitLocker 网络作为解锁*仅*关键保护膜*无法*使用域控制器在其中有部署 Windows 部署服务 (WDS)，因为这样会导致项 scenario 了第一个 DC 要求 Active Directory 和 WDS 工作以解除锁定。 但还原第一个 DC 之前，Active Directory 尚不可用 wds，因此它不能解锁。  
  
     若要确定是否 DC 配置为使用 BitLocker 网络解锁，请检查网络解锁证书已以下注册表项中：  
  
     HKEY_LOCAL_MACHINESoftwarePoliciesMicrosoftSystemCertificatesFVE_NKP  
  
 保持安全程序时处理或还原备份，其中包括 Active Directory 的文件。 附带森林恢复紧急可能会无意中导致顶部俯瞰安全的最佳实践。 有关详细信息，请参阅中标题为"建立域控制器备份和还原战略"的部分[保护 Active Directory 安装和日常操作的最佳实践指南：第二部分](https://technet.microsoft.com/library/bb727066.aspx)。  
  
## <a name="identify-the-current-forest-structure-and-dc-functions"></a>找出当前森林结构和 DC 功能  
 通过识别森林中的所有域确定当前森林结构。 在每个域，特别是具有备份，域控制器和也可能是复制的源的虚拟化的 Dc 使所有 Dc 的列表。 由于在将第一次恢复此域，将最重要齐名 Dc 森林根域。 还原森林根域后，你可以通过使用 Active Directory 单元获得其他域，域控制器，森林中的站点的列表。  
  
 准备表，其中显示在域中，每个直流的功能，如以下示例所示。 这将帮助你恢复为故障预配置的森林后恢复。  
  
|DC 名称|操作系统|FSMO|GC|RODC|备份|DNS|服务器 Core|VM|VM-GenID|  
|-------------|----------------------|----------|--------|----------|------------|---------|-----------------|--------|---------------|  
|DC_1|Windows Server 2012|架构主机，域名主机|是的|不|是的|不|不|是的|是的|  
|DC_2|Windows Server 2012|无|是的|不|是的|是的|不|是的|是的|  
|DC_3|Windows Server 2012|基础结构母版|不|不|不|是的|是的|是的|是的|  
|DC_4|Windows Server 2012|PDC 模拟器不用母版|是的|不|不|不|不|是的|不|  
|DC_5|Windows Server 2012|无|不|不|是的|是的|不|是的|是的|  
|RODC_1|Windows Server 2008 R2|无|是的|是的|是的|是的|是的|是的|不|  
|RODC_2|Windows Server 2008|无|是的|是的|不|是的|是的|是的|不|  
  
 森林中的每个域，识别单个可写 DC 具有该域的 Active Directory 数据库受信任的备份。 当你选择要还原 DC 备份，请务必小心。 如果大约已知的日期和失败的原因，通常建议是使用已发布日期之前的几天备份。  
  
 在此示例中，有四种备份候选：DC_1 DC_2、DC_4，，DC_5。 这些备份候选的还原只有一个。 推荐的 DC 是出于以下原因 DC_5:  
  
-   它满足使用它作为源虚拟化直流复制的要求，前提是运行 Windows Server 2012，在虚拟机监控程序支持 VM-GenerationID，运行软件虚拟 DC 允许要复制（或者，如果不能要复制都可删除）。 在恢复之后 PDC 模拟器角色将取回到服务器，它可以添加到克隆域控制器组域。  
  
-   运行 Windows Server 2012 的完整安装。 运行的服务器 Core 安装 DC 可能不太用于恢复为目标方便。  
  
-   它是一个 DNS 服务器。 因此，DNS 不必重新安装。  
  
> [!NOTE]
>  由于 DC_5 不是一个全球目录服务器，它还具有利用，无需删除后还原全球目录。 但 DC 也是全球目录服务器不可代码因素，因为与 Windows Server 2012 开始，所有 Dc 全球目录服务器默认情况下，并删除和后还原建议作为森林恢复过程的一部分在任何情况下添加全球目录。  
  
## <a name="recover-the-forest-in-isolation"></a>恢复隔离森林  
 首选的方案是在关闭之前的第一个还原 DC 所有可写 Dc 引入返回到生产。 这将确保危险的任何数据不会返回到恢复森林复制。 尤其是务必关闭所有操作的主机角色持有者。  
  
> [!NOTE]
>  这可能是你可以将移动你恢复为每个独立的网络域，同时允许其他 Dc 保持以最小化系统停止在线计划的第一个 DC 的情况。 例如，如果你从失败的模式升级恢复时，你可以选择保留域控制器同时你单独执行恢复步骤生产网络上运行。  
  
 如果你运行的虚拟化域控制器，你可以移到独立于生产网络虚拟网络将在其中执行恢复。 移动到一个单独的网络虚拟化的 Dc 有两个优点：  
  
-   恢复的 Dc 被阻止从再次发生，因为它们独立导致森林恢复的问题。  
  
-   虚拟化 DC 复制可以单独的网络，以便可以运行 Dc 关键大量执行和之前到生产网络导致再次测试。  
  
 如果运行的域控制器上物理硬件，请断开网络电缆的森林根域中还原你计划的第一个 DC。 如果可能，也断开连接的网络电缆的所有其他 Dc。 这可以防止 Dc 复制，如果他们意外启动森林恢复过程。  
  
 在较大树林中跨多个多个位置，它可能很难保证可写的所有 Dc 均已都关闭。 出于此原因，恢复步骤，例如重计算机帐户和 krbtgt 帐户此外元数据清理为-旨在确保，已恢复可写的 Dc 不复制与危险可写 Dc（以防一些仍处于联机状态林中）。  
  
 但是，仅通过让可写 Dc 离线可以你保证不会发生该复制？ 因此，请尽可能地应部署远程管理技术，可帮助你关闭和物理森林恢复过程隔离可写的域控制器。  
  
 Rodc 可以继续操作可写的域控制器在脱机时。 没有其他直流直接将从任何 RODC 复制的任何更改，尤其是，没有架构或配置容器更改，以便他们不会再造成可写 Dc 危险恢复过程。 所有可写的域控制器恢复中和网上后，你应该重新生成所有 Rodc。  
  
 Rodc 将继续允许访问本地缓存在他们各自的站点上并行恢复操作将时的资源。 不会在 RODC 缓存的本地资源将有转发给可写直流身份验证请求。 这些请求将失败，因为可写 Dc 处于离线状态。 诸如密码更改某些操作将还不起作用，直到你恢复可写的域控制器。  
  
 如果你使用的中心分支网络体系结构，你可以首先专注于恢复可写的域控制器在中心的站点。 更高版本，你可以重新生成远程站点中的 Rodc。  
  
## <a name="next-steps"></a>后续步骤
-   [广告森林恢复-先决条件](AD-Forest-Recovery-Prerequisties.md)  
-   [广告森林恢复-在寻找自定义森林恢复套餐](AD-Forest-Recovery-Devising-a-Plan.md)  
- [广告森林恢复-找出问题](AD-Forest-Recovery-Identify-the-Problem.md)
-   [广告森林恢复-确定如何恢复](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [广告森林恢复-执行初始恢复](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [广告森林恢复-过程](AD-Forest-Recovery-Procedures.md)  
-   [广告森林恢复-常见问题](AD-Forest-Recovery-FAQ.md)  
-   [广告森林恢复-恢复单个域中多域森林](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [广告森林恢复-森林恢复与 Windows Server 2003 该域控制器](AD-Forest-Recovery-Windows-Server-2003.md)  
