---
title: "广告森林恢复-执行初始恢复"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: fc2ec09c5b96b76229d532adc6a254c8108d8940
ms.sourcegitcommit: ac73f0f0dca04be731b928183bfdffc97d82c277
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="perform-initial-recovery"></a>执行初始恢复  

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

 此部分中包括以下步骤：  
  
-   [还原在每个域中的第一个可写的域控制器](#Restore-the-first-writeable-domain-controller-in-each-domain)  

-   [重新连接到该网络的每个还原可写的域控制器](#Reconnect-each-restored-writeable-domain-controller-to-a-common-network)  
  
-   [将全球目录添加到域控制器森林根域中](#Add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain)  
  
  
## <a name="restore-the-first-writeable-domain-controller-in-each-domain"></a>还原在每个域中的第一个可写的域控制器  
 开头可写 DC 森林根域中，以便还原第一个 DC 完成此部分中的步骤操作。 森林根域很重要，因为它存储架构管理和企业管理员的组。 它还有助于保持信任层树林中。 此外，森林根域通常包含森林 DNS 命名空间 DNS 根服务器。 因此，Active Directory – 该域集成的 DNS 区域中的所有其他 Dc 树林（其中所需的复制）和全球目录 DNS 资源记录包含 (CNAME) 别名资源记录。  
  
 恢复森林根域后，请重复相同步骤来恢复树林中剩余的域。 你可以同时; 恢复多个域但是，在恢复孩子若要防止任何中信任层次或 DNS 名称分辨率的分隔之前始终恢复家长域。  
  
 对于每个恢复你的域，从备份中还原只有一个可写直流。 这是恢复的最重要的部分，因为 DC 必须具有不已会受到任何导致森林失败数据库。 请务必具有之前引入生产环境全面测试的受信任的备份。  
  
 然后，执行以下步骤。 用于执行特定的步骤的过程中都[广告森林恢复-过程](AD-Forest-Recovery-Procedures.md)。  
  
1.  如果你打算还原物理服务器，请确保网络电缆的目标 DC 未连接，因此未连接到生产网络。 虚拟机，你可以删除该网络适配器，或使用已连接到另一个网络中的网络适配器可在其中测试恢复过程远离生产网络时。  
  
2.  由于这是在域中第一个可写的域控制器，你必须执行的广告 DS 未授权还原和授权的 SYSVOL 还原。 恢复操作必须先完成使用 Active Directory 感知备份和还原应用程序，如 Windows Server 备份（即，否则不应恢复 DC 使用不受支持的方法，如还原 VM 快照）。  
  
     SYSVOL 授权还原需要，因为的 SYSVOL 复制复制从灾难恢复后，必须开始文件夹。 在域中添加的所有后续 Dc 必须重新同步与所选才能将权威该文件夹可以公布文件夹的副本其 SYSVOL 文件夹。  
  
    > [!CAUTION]
    >  执行仅用于在森林根域要还原的第一个 DC SYSVOL 权威（或主要）还原操作。 复制冲突 SYSVOL 数据的导致错误执行其他 Dc SYSVOL 的主要还原操作。  
  
     有两个选项执行的广告 DS 未授权还原和授权的 SYSVOL 还原：  
  
    -   执行完整 server 恢复，然后再强制 SYSVOL 权威同步。 详细步骤，请参阅[执行完整服务器恢复](AD-Forest-Recovery-Perform-a-Full-Recovery.md)和[执行 DFSR 复制 SYSVOL 权威同步](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)。  
  
    -   执行状态系统还原后跟完全服务器恢复。 此选项要求你创建的备份这两种类型提前：完整服务器备份并系统状态备份。 详细步骤，请参阅[执行完整服务器恢复](AD-Forest-Recovery-Perform-a-Full-Recovery.md)和[执行权威的 Active Directory 域服务还原](AD-Forest-Recovery-Nonauthoritative-Restore.md)。  
  
3.  还原，并且可写直流重启后，可验证失败不会影响域控制器上的数据。 如果已损坏 DC 数据，然后重复步骤 2 具有不同的备份。  
  
     如果要还原的域控制器驻留操作主机角色，你可能需要添加以下注册表项，以避免广告 DS 完成可写的目录分区复制的之前不可用：  
  
     **HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Repl 执行初始同步**  
  
     使用的数据类型创建条目**REG_DWORD**，值为**0**。 森林恢复完全之后，你可以重置为此项的值**1**，这需要重新启动并保持操作主机角色让成功的域控制器广告 DS 归巢站复制其已知的副本合作伙伴之前，它将作为域控制器公布自己并开始提供给客户服务。 有关详细信息，请参阅知识库文章初始同步要求，[305476](https://support.microsoft.com/kb/305476)。  
  
     仅在还原并验证数据之后和之前到生产网络加入该计算机，请转到下面的步骤。  
  
4.  如果你怀疑树林失败与网络入侵或恶意攻击，请重置所有管理的帐户，包括成员企业管理员、域管理员，架构管理员，服务器运营商、帐户运算符组等帐户的密码。 重置管理的帐户的密码的应之前额外的域控制器在安装过程中的下一个阶段森林恢复来完成。  
  
5.  在第一天中森林根域中，已恢复的 DC 获取所有的域范围内和树林操作主角色。 需要企业管理员和方案管理员凭据占用树林操作主机角色。  
  
     在每个孩子域，占用全域操作主机角色。 尽管你可能会保留操作主机角色还原的 DC 仅暂时上, 占用这些角色可确保你的有关 DC 承载着它们在此时森林恢复过程中。 作为后恢复过程的一部分，可以根据需要重新操作主机角色。 有关占用操作主角色的详细信息，请参阅[占用操作主角色](AD-forest-recovery-seizing-operations-master-role.md)。 有关位置操作主机角色建议，请参阅[操作主机是什么？](https://technet.microsoft.com/en-us/library/cc779716.aspx).  
  
6.  清除你不还原从备份 (在此第一 DC 除外域所有可写 Dc) 森林根域中的所有其他可写 Dc 的元数据。 如果你使用的版本的 Active Directory 用户和计算机或 Active Directory 的站点和服务，包含在 Windows Server 2008 或更高版本或 RSAT Windows Vista 或更高版本，当您删除 DC 对象元数据清理将自动执行。 此外，服务器对象计算机已删除 DC 对象也会删除和自动。 有关详细信息，请参阅[清洁的元数据中删除可写 Dc](AD-Forest-Recovery-Cleaning-Metadata.md)。  
  
     如果在另一个站点 DC 上安装广告 DS 清理元数据阻止可能复制-各自的对象。 潜在，这还还可以节省知识一致性检查 (KCC) 时，域控制器自行可能不会出现创建复制链接的过程。 此外，作为元数据清理资源记录 DC 定位器 DNS 域中的所有其他 Dc 将从 DNS 被删除。  
  
     域中的所有其他 Dc 元数据将被删除，直到此域控制器，就像恢复之前, RID 主机不承担 RID 主机角色和因此将无法处理新 Rid。 你可能会看到事件 ID 16650 在系统日志中事件查看器指示此失败，但你应看到事件 ID 16648 指示成功一段时间后，你有清理元数据。  
  
7.  如果你有存储在广告 DS 中的 DNS 区域，确保本地 DNS 服务器服务已还原直流上安装和运行。 如果此 DC 未 DNS 服务器森林发生故障之前，你必须安装并配置 DNS 服务器。  
  
    > [!NOTE]
    >  Windows Server 2008 还原域控制器耗尽，需要知识库文章中安装修补程序[975654](https://support.microsoft.com/kb/975654)或服务器暂时连接到网络独立才能安装 DNS 服务器。 不需要任何其他版本的 Windows Server 修补程序。  
  
     森林根域中，在配置 IP 地址（或回送地址，如 127.0.0.1）作为的首选的 DNS 服务器还原的直流。 你可以在本地网络 (LAN) 适配器 TCP/IP 属性配置此设置。 这是森林中的第一个 DNS 服务器。 有关详细信息，请参阅[配置 TCP/IP 使用 DNS](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx)。  
  
     在每个孩子域中，将配置还原的直流森林根域中的第一个 DNS 服务器的 IP 地址它的首选 DNS 服务器。 你可以在 TCP/IP 属性 LAN 适配器的配置此设置。 有关详细信息，请参阅[配置 TCP/IP 使用 DNS](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx)。  
  
     _Msdcs 和域 DNS 区域中删除元数据清理后不存在的 Dc NS 的记录。 检查 SRV 记录清理 Dc 的已被删除。 若要帮助加快 DNS SRV 录制删除，运行：  
  
    ```  
    nltest.exe /dsderegdns:server.domain.tld  
    ```  
  
8.  通过 100000 筹集可用 RID 池的值。 有关详细信息，请参阅[引发可用 RID 池值](AD-Forest-Recovery-Raise-RID-Pool.md)。 如果你有一个原因认为是特定的情况下不足，无法通过 100000 引发不用池，你应该确定仍安全地使用的最低增加。 Rid 将不能使用向上不必要的有限的资源。  
  
     如果之后的备份中还原你使用的时间在域中创建新的安全原则，这些安全主体可能会在某些对象拥有访问权限。 这些安全主体不再存在后恢复因为恢复已还原到备份;但是，他们的访问权限可能仍然存在。 如果恢复后无法引发可用 RID 池时，新的用户对象创建森林恢复可能获取相同的安全 Id (Sid) 后，并可能有权访问这些对象，这不最初预期花费。  
  
     为了说明，请考虑名为 Amy 简介中提到的新员工的示例。 因为它用于还原域备份之后创建张瑾的用户对象不再存在后还原操作。 但是，任何已分配给用户对象的访问权限可能会保持后还原操作。 如果该用户对象 SID 重新分配到新的对象恢复操作后，新的对象会获得这些的访问权限。  
  
9. 使失效当前 RID 池。 当前 RID 池系统状态恢复后失效。 但是如果没有执行状态系统还原，则需要失效若要防止还原的直流重新从 RID 池分配创建备份时发出 Rid 当前 RID 池。 有关详细信息，请参阅[失效当前 RID 池](AD-Forest-Recovery-Invaildate-RID-Pool.md)。  
  
    > [!NOTE]
    >  尝试与 SID 创建对象，使失效 RID 池后的第一次，你将收到的错误。 尝试创建一个触发请求 RID 新池。 该操作的重试成功，因为将分配 RID 新池。  
  
10. 两次重置此 DC 计算机帐户密码。 有关详细信息，请参阅[域控制器计算机帐户密码重置](AD-Forest-Recovery-Reset-Computer-Account-DC.md)。  
  
11. 两次重置 krbtgt 密码。 有关详细信息，请参阅[krbtgt 密码重置](AD-Forest-Recovery-Resetting-the-krbtgt-password.md)。  
  
     由于 krbtgt 密码历史记录为两个密码，重置密码，两次努力，从此密码历史记录中删除原始（故障）密码。  
  
    > [!NOTE]
    >  如果森林恢复安全漏洞的响应，你可能会重置信任密码。 有关详细信息，请参阅[重置密码等信任一侧的信任](AD-Forest-Recovery-Reset-Trust.md)。  
  
12. 如果还原的直流了发生故障之前全球目录服务器森林具有多个域，清除**全球目录**各自的属性，若要从 DC 删除全球目录中的复选框。 此规则在于具有只需一个域森林的常见情况。 在此情况下，它不需要删除全球目录。 有关详细信息，请参阅[删除全球目录](AD-Forest-Recovery-Remove-GC.md)。  
  
     通过从是更多的备份中还原全球目录最近比其他备份用来还原域控制器在其他域中，你可能会引入延迟对象。 请考虑以下示例。 在一个域中，DC1 是从备份中还原次 T1 拍摄。 在 B 域中，从次 T2 拍摄全局的目录备份还原 DC2。 假设 T2 器比 T1，并且 T1 和 T2 之间创建了一些对象。 在还原这些 Dc 后，DC2，为全球的目录，保存较新数据域队部分副本比域 A 保证金本身。 DC2，在本例中，保存延迟对象，因为这些对象上 DC1 没有。  
  
     延迟对象存在可能导致问题。 例如，电子邮件不可能给其用户对象移动域之间的用户提供。 你将过期的 DC 或全球目录 server 联机后，用户对象这两种情况下将显示在全球目录中。 这两个对象具有相同电子邮件地址。因此，不能提供的电子邮件。  
  
     第二个问题是，不会再存在一个用户帐户可能仍会显示全局地址列表中。 第三个问题是，不会再存在一个通用组可能仍会出现在用户访问标记。  
  
     如果你未还原已全球目录直流，或者无意中，或者是因为这是你信任的作对备份，我们建议你避免中通过禁用显示器的全球目录很快在完成恢复操作后逗留对象。 禁用显示器的全球目录标志会导致计算机丢失所有其部分副本（分区）和 relegating 本身到常规 DC 状态。  
  
13. 配置 Windows 时间服务。 在森林根域中，将配置 PDC 模拟器同步外部时间来源的时间。 有关详细信息，请参阅[PDC 模拟器森林根域中上配置 Windows 时间服务](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29)。  
  
 

## <a name="reconnect-each-restored-writeable-domain-controller-to-a-common-network"></a>重新连接到公共网络的每个还原可写的域控制器  
 在此阶段你应该拥有还原一个直流（和执行恢复步骤）森林根域中和每个剩余的域。 这些 Dc 加入独立于环境其余部分的公共网络并验证森林健康和复制才能完成以下步骤。  
  
> [!NOTE]
>  当物理 Dc 加入隔离网络后时，你可能需要更改它们的 IP 地址。 因此，DNS 记录的 IP 地址将错误。 全球目录服务器不可用，因为将无法为 DNS 安全的动态更新。 虚拟 Dc 很更有用，在此情况下因为他们可以到了新的虚拟网络加入，而无需更改它们的 IP 地址。 这是一个原因虚拟 Dc 建议作为第一个域控制器森林恢复过程要还原的原因。  
  
 验证后到生产网络加入域控制器，并完成的验证森林复制运行状况的步骤。  
  
-   若要解决名称分辨率，创建 DNS 委派记录并配置根据需要 DNS 转移和根提示。 运行**repadmin /replsum**检查 Dc 之间复制。  
  
-   如果还原的 DC 不直接复制合作伙伴，复制恢复将更快通过创建临时连接它们之间的对象。  
  
-   若要验证元数据清理，运行**Repadmin /viewlist \ ***森林中的所有 Dc 的列表。 运行**Nltest /DCList:***< domain\ >*域中的所有 Dc 的列表。  
  
-   若要检查 DC 和 DNS 运行状况，请森林中的所有域控制器上运行报告错误 DCDiag /v。  
  
  

## <a name="add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain"></a>将全球目录添加到域控制器森林根域中  
 全球目录，才能进行这些和其他原因：  
  
-   若要启用的用户登录。  
  
-   若要启用网络登录服务在每个孩子域域控制器注册并删除根域中的 DNS 服务器上的记录上运行。  
  
 尽管优先的森林根 DC 变为全球目录，就可以选择任何还原 Dc 变得全球目录。  
  
> [!NOTE]
>  完成了在完全同步森林中的所有目录分区之前，不会为全球的目录服务器公布 DC。 因此，应强制 DC 与每个森林中的恢复 Dc 进行复制。  
>   
>  监控目录服务中的事件日志事件查看器事件 ID 1119，这表明该 DC 全球目录服务器，或验证以下注册表项具有 1 的值：  
>   
>  **完成 HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Global 目录提升**  
  
 有关详细信息，请参阅[添加全球目录](AD-Forest-Recovery-Add-GC.md)。  
  
 在这个阶段，你应该拥有具有一个 DC 的每个域稳定林和一个全球目录树林中。 你应该使你只需还原 Dc 的新备份。 现在，你可以开始通过安装广告 DS 重新部署森林中的其他 Dc。  

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
  
