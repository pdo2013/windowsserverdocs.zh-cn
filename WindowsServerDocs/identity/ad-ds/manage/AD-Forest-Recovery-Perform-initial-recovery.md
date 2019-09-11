---
title: AD 林恢复-执行初始恢复
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: ca942691a88465061ebbde2b78314844a694fbf2
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868201"
---
# <a name="perform-initial-recovery"></a>执行初始恢复  

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

本部分包括以下步骤：  

- [还原每个域中的第一个可写域控制器](#restore-the-first-writeable-domain-controller-in-each-domain)  
- [将每个还原的可写域控制器重新连接到网络](#reconnect-each-restored-writeable-domain-controller-to-a-common-network)  
- [将全局编录添加到目录林根级域中的域控制器](#add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain)  

## <a name="restore-the-first-writeable-domain-controller-in-each-domain"></a>还原每个域中的第一个可写域控制器  

从目录林根级域中的可写 DC 开始，完成此部分中的步骤，以便还原第一个 DC。 目录林根级域很重要，因为它存储 Schema Admins 和 Enterprise Admins 组。 它还有助于维护林中的信任层次结构。 此外，目录林根级域通常保存林的 DNS 命名空间的 DNS 根服务器。 因此，该域的 Active Directory 集成 DNS 区域包含林中所有其他 Dc 的别名（CNAME）资源记录（复制需要）和全局编录 DNS 资源记录。 
  
恢复目录林根级域后，重复相同的步骤来恢复林中的其余域。 可以同时恢复多个域;但是，在恢复子域之前始终恢复父域，以防止信任层次结构中的任何中断或 DNS 名称解析。 
  
对于恢复的每个域，只从备份还原一个可写 DC。 这是恢复的最重要部分，因为 DC 必须有一个数据库，该数据库未受到导致林失败的任何影响。 在将受信任的备份引入生产环境之前，必须对其进行全面测试，这一点非常重要。 
  
然后执行以下步骤。 [AD 林恢复-过程](AD-Forest-Recovery-Procedures.md)中提供了执行某些步骤的过程。 
  
1. 如果你计划还原物理服务器，请确保目标 DC 的网络电缆未连接，因此未连接到生产网络。 对于虚拟机，你可以删除网络适配器，或使用附加到另一个网络的网络适配器，在该网络上，你可以在与生产网络隔离的情况下测试恢复过程。 
  
2. 由于这是域中的第一个可写 DC，因此你必须对 AD DS 执行非权威还原，并执行 SYSVOL 的权威还原。 必须使用 Active Directory 感知的备份和还原应用程序（例如 Windows Server 备份，而不能使用不受支持的方法（如还原 VM 快照）来还原 DC，才能完成还原操作。 
   - 需要对 SYSVOL 进行权威还原，因为在从灾难中恢复后，必须启动 SYSVOL 已复制文件夹的复制。 添加到域中的所有后续 Dc 必须重新同步其 SYSVOL 文件夹，其中包含已被选为权威文件夹的文件夹副本，然后才能播发该文件夹。 

   > [!CAUTION]
   > 仅对要在目录林根级域中还原的第一个 DC 执行 SYSVOL 的权威（或主）还原操作。 在其他 Dc 上错误地执行 SYSVOL 的主还原操作会导致 SYSVOL 数据的复制冲突。 

   - 有两个选项可执行 AD DS 的非权威还原，以及 SYSVOL 的权威还原：  
   - 执行完整服务器恢复，并强制执行 SYSVOL 的权威同步。 有关详细过程，请参阅[执行完整服务器恢复](AD-Forest-Recovery-Perform-a-Full-Recovery.md)和[执行 DFSR 复制的 SYSVOL 的权威同步](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)。 
   - 执行完整服务器恢复，然后执行系统状态还原。 此选项要求您预先创建这两种类型的备份：完整的服务器备份和系统状态备份。 有关详细过程，请参阅[执行完整服务器恢复](AD-Forest-Recovery-Perform-a-Full-Recovery.md)和[执行 Active Directory 域服务的非权威还原](AD-Forest-Recovery-Nonauthoritative-Restore.md)。 
  
3. 还原并重新启动可写 DC 后，验证失败是否不会影响 DC 上的数据。 如果 DC 数据已损坏，请使用不同的备份重复步骤2。 
   - 如果还原的域控制器承载操作主机角色，则你可能需要添加以下注册表项，以避免在完成对可写目录分区的复制之后 AD DS 不可用：  

      **HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Repl 执行初始同步**  
  
      创建数据类型为**REG_DWORD**且值为**0**的条目。 完全恢复林后，可以将此项的值重置为**1**，这需要一个域控制器，该控制器会重新启动并保留操作主机角色，以便成功 AD DS 入站和出站复制与其已知副本合作伙伴在将自身广告为域控制器并开始为客户端提供服务之前。 有关初始同步要求的详细信息，请参阅知识库文章[305476](https://support.microsoft.com/kb/305476)。 
  
      仅在还原并验证数据之后以及将此计算机加入到生产网络之前，才继续执行后续步骤。 
  
4. 如果怀疑林范围的故障与网络入侵或恶意攻击相关，请重置所有管理帐户的帐户密码，包括 Enterprise Admins、Domain Admins、Schema Admins、Server Operators、AccountOperators 组，等等。 在林恢复的下一个阶段安装其他域控制器之前，应先完成重置管理帐户密码。 
5. 在目录林根级域中的第一个还原 DC 上，获取所有全域性和全林性操作主机角色。 需要企业管理员和架构管理员凭据才能占用林范围的操作主机角色。 
  
     在每个子域中，占用域范围内的操作主机角色。 尽管你可能只是暂时保留已还原 DC 上的操作主机角色，但占用这些角色可确保你在林恢复过程中的哪个 DC 上托管这些角色。 作为恢复后过程的一部分，你可以根据需要重新分发操作主机角色。 有关占用操作主机角色的详细信息，请参阅[占用操作主机角色](AD-forest-recovery-seizing-operations-master-role.md)。 有关在何处放置操作主机角色的建议，请参阅[什么是操作主机？](https://technet.microsoft.com/library/cc779716.aspx)。 
  
6. 清除不是从备份还原的目录林根级域中所有其他可写 Dc 的元数据（除此第一个 DC 外，域中的所有可写 Dc 除外）。 如果使用 Windows Server 2008 或更高版本附带的 Active Directory 用户和计算机或 Active Directory 站点和服务的版本，或 Windows Vista 或更高版本的 RSAT，则在删除 DC 对象时将自动执行元数据清除。 此外，还会自动删除已删除 DC 的服务器对象和计算机对象。 有关详细信息，请参阅[清除已删除可写 dc 的元数据](AD-Forest-Recovery-Cleaning-Metadata.md)。 
  
     如果将 AD DS 安装在另一个站点中的 DC 上，则清除元数据可防止可能出现的 NTDS 设置对象重复。 这可能还会保存知识一致性检查器（KCC）在 Dc 本身可能不存在时创建复制链接的过程。 此外，在元数据清理过程中，将从 DNS 中删除域中所有其他 Dc 的 DC 定位程序 DNS 资源记录。 
  
     在删除域中所有其他 Dc 的元数据之前，此 DC （如果在恢复之前是 RID 主机）不会采用 RID 主机角色，因此将无法颁发新 Rid。 你可能会在系统日志中看到事件 ID 16650，事件查看器指示此失败的原因，但你应看到事件 ID 16648，指示在清除元数据后的成功。 
  
7. 如果你的 DNS 区域存储在 AD DS 中，请确保本地 DNS 服务器服务已安装并在已还原的 DC 上运行。 如果在林失败之前，此 DC 不是 DNS 服务器，则必须安装和配置 DNS 服务器。 
  
    > [!NOTE]
    > 如果还原的 DC 运行 Windows Server 2008，则需要在知识库文章[975654](https://support.microsoft.com/kb/975654)中安装此修补程序，或者暂时将服务器连接到隔离的网络，以便安装 DNS 服务器。 任何其他版本的 Windows Server 都不需要此修补程序。 
  
     在目录林根级域中，使用其自己的 IP 地址（或环回地址（如127.0.0.1））将还原的 DC 配置为其首选 DNS 服务器。 你可以在局域网（LAN）适配器的 TCP/IP 属性中配置此设置。 这是林中的第一个 DNS 服务器。 有关详细信息，请参阅[将 Tcp/ip 配置为使用 DNS](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx)。 
  
     在每个子域中，用目录林根级域中的第一个 DNS 服务器的 IP 地址配置还原的 DC 作为其首选 DNS 服务器。 可以在 LAN 适配器的 TCP/IP 属性中配置此设置。 有关详细信息，请参阅[将 Tcp/ip 配置为使用 DNS](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx)。 
  
     在 "_msdcs" 和 "域" DNS 区域中，删除在清除元数据后不再存在的 Dc 的 NS 记录。 检查是否已删除清理的 Dc 的 SRV 记录。 若要加快删除 DNS SRV 记录的速度，请运行：  
  
    ```  
    nltest.exe /dsderegdns:server.domain.tld  
    ```  
  
8. 将可用 RID 池的值提升100000。 有关详细信息，请参阅[提高可用 RID 池的值](AD-Forest-Recovery-Raise-RID-Pool.md)。 如果有理由相信，100000使 RID 池不足以满足你的特定情况，你应该确定仍然可安全使用的最小增长。 Rid 是一种有限的资源，不应不必要地使用。 
  
     如果在用于还原的备份后在域中创建了新的安全主体，则这些安全主体可能对某些对象具有访问权限。 恢复后这些安全主体不再存在，因为恢复已还原到备份;但是，它们的访问权限可能仍然存在。 如果在还原后未引发可用 RID 池，则在林恢复之后创建的新用户对象可能会获得相同的安全 Id （Sid），并且可能有权访问这些对象，这些对象最初并不是预期的。 
  
     为了说明这一点，请考虑简介中提到的名为 "张瑾雯" 的新员工的示例。 在还原操作之后，张瑾雯的用户对象已不存在，因为它是在用于还原域的备份之后创建的。 但是，在还原操作之后，分配给该用户对象的任何访问权限都可能会保持。 如果在执行还原操作之后，该用户对象的 SID 已重新分配给新对象，则新的对象将获得这些访问权限。 
  
9. 使当前 RID 池无效。 当前 RID 池在系统状态还原后失效。 但是，如果未执行系统状态还原，则当前 RID 池需要失效，以防止还原的 DC 从创建备份时分配的 RID 池中重新颁发 Rid。 有关详细信息，请参阅[使当前 RID 池失效](AD-Forest-Recovery-Invaildate-RID-Pool.md)。 
  
    > [!NOTE]
    > 在使 RID 池无效后，首次尝试使用 SID 创建对象时，会收到错误。 尝试创建对象会触发对新 RID 池的请求。 重试操作成功，因为将分配新的 RID 池。 
  
10. 重置此 DC 的计算机帐户密码两次。 有关详细信息，请参阅[重置域控制器的计算机帐户密码](AD-Forest-Recovery-Reset-Computer-Account-DC.md)。 
  
11. 重置 krbtgt 密码两次。 有关详细信息，请参阅[重置 krbtgt 密码](AD-Forest-Recovery-Resetting-the-krbtgt-password.md)。 
  
     由于 krbtgt 密码历史记录是两个密码，请重置密码两次，以从密码历史记录中删除原始（prefailure）密码。 
  
    > [!NOTE]
    > 如果林恢复需要响应安全漏洞，则还可以重置信任密码。 有关详细信息，请参阅[重置信任一方的信任密码](AD-Forest-Recovery-Reset-Trust.md)。 
  
12. 如果林中有多个域，并且还原的 DC 是发生故障之前的全局编录服务器，请清除 "NTDS 设置" 属性中的 "**全局编录**" 复选框，以从 DC 中删除全局编录。 此规则的例外情况是仅有一个域的林的常见情况。 在这种情况下，不需要删除全局编录。 有关详细信息，请参阅[删除全局编录](AD-Forest-Recovery-Remove-GC.md)。 
  
     通过从备份中还原全局编录，该备份比其他域中用于还原 Dc 的其他备份要新。 请考虑以下示例。 在域 A 中，DC1 是从在时间 T1 拍摄的备份中还原的。 在域 B 中，DC2 从在时间 T2 拍摄的全局编录备份还原。 假设 T2 比 T1 更近，并且某些对象是在 T1 和 T2 之间创建的。 还原这些 Dc 后，DC2 （这是一个全局编录）会保留域 A 的部分副本的更新数据，而不是域 A 本身。 在这种情况下，DC2 保存了延迟对象，因为 DC1 上不存在这些对象。 
  
     延迟对象的存在可能导致问题。 例如，可能无法将电子邮件发送到用户对象在域之间移动的用户。 使过时的 DC 或全局编录服务器重新联机后，该用户对象的两个实例都将显示在全局目录中。 这两个对象具有相同的电子邮件地址;因此无法传递电子邮件。 
  
     第二个问题是，不存在的用户帐户可能仍会出现在全局地址列表中。 第三个问题是：已不存在的通用组可能仍会出现在用户的访问令牌中。 
  
     如果你确实还原了作为全局编录的 DC，无论是不小心还是因为这是你信任的孤立备份，我们建议你在执行还原操作后立即禁用全局编录来阻止发生延迟对象。完成. 禁用全局编录标志将导致计算机丢失其所有部分副本（分区），并 relegating 自身为常规 DC 状态。 
  
13. 配置 Windows 时间服务。 在目录林根级域中，将 PDC 仿真器配置为从外部时间源同步时间。 有关详细信息，请参阅在[林根域中的 PDC 模拟器上配置 Windows 时间服务](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29)。 
  
## <a name="reconnect-each-restored-writeable-domain-controller-to-a-common-network"></a>将每个还原的可写域控制器重新连接到公共网络

在此阶段，你应在目录林根级域和每个剩余域中还原一个 DC （和执行恢复步骤）。 将这些 Dc 加入到与环境的其余部分隔离的公共网络，并完成以下步骤以验证林的运行状况和复制。

> [!NOTE]
> 将物理 Dc 加入到隔离的网络时，可能需要更改其 IP 地址。 因此，DNS 记录的 IP 地址将会出错。 由于全局编录服务器不可用，因此 DNS 的安全动态更新将会失败。 在这种情况下，虚拟 Dc 更有利，因为它们可以加入到新的虚拟网络，而不会更改其 IP 地址。 这就是为什么建议在林恢复期间还原第一个域控制器的一个原因。 
  
验证后，将 Dc 加入生产网络并完成验证林复制运行状况的步骤。

- 若要修复名称解析，请根据需要创建 DNS 委托记录并配置 DNS 转发和根提示。 运行**repadmin/replsum**以检查域控制器之间的复制。 
- 如果还原的 DC 不是直接复制伙伴，则通过在它们之间创建临时连接对象，可以更快地进行复制恢复。 
- 若要验证元数据清除，请运行**Repadmin/viewlist \\** * 获取林中所有 dc 的列表。 运行**Nltest/DCList：** *< 域\>*  ，获取域中所有 dc 的列表。 
- 若要检查 DC 和 DNS 运行状况，请运行 DCDiag/v 报告林中所有 Dc 上的错误。 

## <a name="add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain"></a>将全局编录添加到目录林根级域中的域控制器

需要全局编录，原因如下：  
  
- 为用户启用登录。 
- 若要启用在每个子域中的 Dc 上运行的 Net Logon 服务，在根域中的 DNS 服务器上注册和删除记录。 
  
虽然目录林根 DC 可以成为全局编录，但可以选择任何已还原的 Dc 来成为全局编录。 
  
> [!NOTE]
> 在完全同步林中的所有目录分区之前，不会将 DC 播发为全局编录服务器。 因此，应强制 DC 与林中的每个已还原 Dc 进行复制。 
>
> 监视事件查看器事件 ID 1119 的目录服务事件日志，该日志指示此 DC 是全局编录服务器，或验证以下注册表项的值是否为1：  
>
> **HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Global 目录升级完成**  
  
有关详细信息，请参阅[添加全局编录](AD-Forest-Recovery-Add-GC.md)。 
  
在此阶段，你应该具有一个稳定的林，其中每个域有一个 DC，并且林中有一个全局编录。 应为刚还原的每个 Dc 创建新的备份。 你现在可以通过安装 AD DS 来开始重新部署林中的其他 Dc。 

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
