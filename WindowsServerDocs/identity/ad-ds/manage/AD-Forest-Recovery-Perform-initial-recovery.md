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
ms.openlocfilehash: 9883d337520c3920f8638ddfe5f6bd393e31fd2f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442858"
---
# <a name="perform-initial-recovery"></a>执行初始恢复  

>适用于：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

本部分包括以下步骤：  

- [还原每个域中的第一个可写域控制器](#restore-the-first-writeable-domain-controller-in-each-domain)  
- [重新连接到网络的每个还原的可写域控制器](#reconnect-each-restored-writeable-domain-controller-to-a-common-network)  
- [将全局编录添加到林根级域中的域控制器](#add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain)  

## <a name="restore-the-first-writeable-domain-controller-in-each-domain"></a>还原每个域中的第一个可写域控制器  

从开始目录林根域中的可写 DC，若要还原第一个域控制器完成此部分中的步骤。 目录林根域很重要，因为它将存储的 Schema Admins 和 Enterprise Admins 组。 它还有助于保持在林中的信任层次结构。 此外，目录林根域通常还赋有林 DNS 命名空间的 DNS 根服务器。 因此，该域的 Active Directory 集成 DNS 区域包含别名 (CNAME) 资源记录为林 （这是用于复制所必需） 和全局编录 DNS 资源记录中的所有其他 Dc。 
  
恢复目录林根域后，重复相同的步骤来恢复在林中的其余域。 可以同时; 恢复多个域但是，在恢复子以防止信任层次结构或 DNS 名称解析中的任何中断之前始终恢复父域。 
  
对于您恢复每个域，从备份中还原只有一个可写 DC。 这是因为 DC 必须有一个数据库，具有不已受导致林无法恢复的最重要的一部分。 请务必具有之前引入生产环境进行全面测试的受信任的备份。 
  
然后执行以下步骤。 执行特定步骤的过程中都是[AD 林恢复-过程](AD-Forest-Recovery-Procedures.md)。 
  
1. 如果您计划还原的物理服务器，确保 DC 未附加的目标，因此未连接到生产网络的网络电缆。 对于虚拟机，可以删除网络适配器，或使用附加到另一个网络的网络适配器可在其中测试恢复过程时生产网络隔离。 
  
2. 由于这是域中的第一个可写 DC，必须执行 AD DS 的非权威还原和 SYSVOL 的权威还原。 还原操作必须通过使用 Active Directory 感知的备份和还原应用程序，如 Windows Server Backup （也就是说，您应还原 DC 使用不受支持的方法，如还原 VM 快照）。 
   - SYSVOL 的权威还原需要，因为复制 SYSVOL 的复制从灾难恢复后，必须在启动文件夹。 在域中添加所有后续的域控制器必须重新同步已选择要成为权威之前可以播发文件夹的文件夹的副本及其 SYSVOL 文件夹。 

   > [!CAUTION]
   > 执行仅对要还原在目录林根域中的第一个 DC 的 SYSVOL 的权威 （或主键） 的还原操作。 错误地执行其他域控制器上的 SYSVOL 的主还原操作会导致复制的 SYSVOL 数据的冲突。 

   - 有两个选项执行 AD DS 的非权威还原和 SYSVOL 的权威还原：  
   - 执行整个服务器恢复，然后强制执行 SYSVOL 的权威同步。 有关详细过程，请参阅[执行整个服务器恢复](AD-Forest-Recovery-Perform-a-Full-Recovery.md)并[执行 DFSR 复制的 SYSVOL 的权威同步](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)。 
   - 执行整个服务器恢复系统状态还原后跟。 此选项要求提前创建这两种类型的备份： 完整服务器备份和系统状态备份。 有关详细过程，请参阅[执行整个服务器恢复](AD-Forest-Recovery-Perform-a-Full-Recovery.md)并[执行 Active Directory 域服务非权威还原](AD-Forest-Recovery-Nonauthoritative-Restore.md)。 
  
3. 还原并重新启动的可写 DC 后，验证失败未影响 DC 上的数据。 如果 DC 数据已损坏，然后使用其他备份重复步骤 2。 
   - 如果还原的域控制器托管操作主机角色，你可能需要添加以下注册表项以避免变得不可用，直到它完成的可写目录分区复制 AD DS:  

      **HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Repl Perform Initial Synchronizations**  
  
      创建具有数据类型的条目**REG_DWORD**并将值**0**。 完全恢复林后，可以重置为此项的值**1**，后者要求重新启动域控制器，持有操作主机角色具有成功的 AD DS 的入站和出站复制具有其已知副本合作伙伴之前它将公布自己为域控制器并开始为客户端提供服务。 有关初始同步要求，请参阅知识库文章的详细信息[305476](https://support.microsoft.com/kb/305476)。 
  
      仅还原和验证数据之后，将此计算机加入到生产网络之前，请转到下一个步骤。 
  
4. 如果您怀疑全林性失败与网络入侵或恶意攻击，重置所有管理帐户，包括 Enterprise Admins、 Domain Admins、 Schema Admins、 Server Operators 帐户的成员的帐户密码运算符组，依次类推。 其他域控制器安装在下一阶段将林恢复过程之前应完成的管理帐户密码重置。 
5. 在目录林根域中的第一个还原 DC，占用所有全域性和全林性的操作主机角色。 Enterprise Admins 和 Schema Admins 凭据所需占用林范围操作主机角色。 
  
     在每个子域，占用域范围操作主机角色。 尽管您可能会保留操作主机角色只是暂时，还原的 DC 上占用这些角色可确保您有关哪个 DC 托管它们在此时林恢复过程中。 一样恢复后的过程的一部分，可以将操作主机角色重新分发它根据需要。 有关占用操作主机角色的详细信息，请参阅[占用操作主机角色](AD-forest-recovery-seizing-operations-master-role.md)。 有关在何处放置操作主机角色的建议，请参阅[什么是操作主机？](https://technet.microsoft.com/library/cc779716.aspx)。 
  
6. 清除您不会还原从备份 (除此第一个域控制器的域中所有可写 Dc) 目录林根域中的所有其他可写 Dc 的元数据。 如果您使用的 Active Directory 用户和计算机或 Active Directory 站点和服务包含在 Windows Server 2008 或更高版本或 Windows Vista 的 RSAT 版本或更高版本，元数据清除自动执行时删除 DC 对象。 此外，服务器对象和已删除的 DC 的计算机对象也会删除自动。 有关详细信息，请参阅[清理的元数据删除可写 Dc](AD-Forest-Recovery-Cleaning-Metadata.md)。 
  
     如果 AD DS 安装在不同站点中的 DC 上清理元数据可防止可能重复的 NTDS 设置对象。 有可能，这还可以将保存知识一致性检查器 (KCC) 时可能不在域控制器本身创建复制链接的过程。 此外，作为一部分的元数据清除，将从 DNS 删除域中的所有其他域控制器的 DC 定位程序 DNS 资源记录。 
  
     直到删除域中的所有其他域控制器的元数据，此 DC，就像之前恢复，RID 主机将不会假定 RID 主机角色，因此不将可以颁发新的 Rid。 可能会看到事件 ID 16650 系统日志中，该值指示此失败的事件查看器中，但您应该看到事件 ID 16648 表示成功一段时间后在清理完元数据。 
  
7. 如果必须存储在 AD DS 的 DNS 区域，请确保本地 DNS 服务器服务已安装并已还原的 DC 上运行。 如果此 DC 不是 DNS 服务器在林发生故障之前，必须安装并配置 DNS 服务器。 
  
    > [!NOTE]
    > 如果已还原的 DC 在运行 Windows Server 2008，则需要安装知识库文章中描述的修补程序[975654](https://support.microsoft.com/kb/975654)或服务器暂时连接到隔离的网络才能安装 DNS 服务器。 不需要任何其他版本的 Windows Server 修补程序。 
  
     在林根域中，作为其首选 DNS 服务器配置已还原的 DC 使用自己的 IP 地址 （或环回地址，例如 127.0.0.1）。 局域网 (LAN) 适配器的 TCP/IP 属性中，可以配置此设置。 这是林中的第一个 DNS 服务器。 有关详细信息，请参阅[配置 TCP/IP，使用 DNS](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx)。 
  
     在每个子域，配置已还原的 DC 目录林根域中的第一个 DNS 服务器的 IP 地址作为其首选 DNS 服务器。 将 LAN 适配器的 TCP/IP 属性中，可以配置此设置。 有关详细信息，请参阅[配置 TCP/IP，使用 DNS](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx)。 
  
     _Msdcs 和域 DNS 区域中删除不再存在元数据清理后的 Dc 的 NS 记录。 检查是否已删除的清理 Dc 的 SRV 记录。 若要帮助加快 DNS SRV 记录删除，请运行：  
  
    ```  
    nltest.exe /dsderegdns:server.domain.tld  
    ```  
  
8. 100,000 引发可用 RID 池的值。 有关详细信息，请参阅[引发的值可用 RID 池](AD-Forest-Recovery-Raise-RID-Pool.md)。 如果您有理由相信，100,000 引发 RID 池是不足，无法在特定情况下，应确定仍保持安全，若要使用的最小增长。 Rid 是不应使用了不必要的有限资源。 
  
     如果新的安全主体在域中创建用于还原备份的时间后，这些安全主体可能会对某些对象具有访问权限。 这些安全主体不存在恢复后因为恢复已还原备份;但是，仍可能存在其访问权限。 如果可用的 RID 池不会引发在还原后，新用户对象林恢复可能会获得相同的安全 Id (Sid) 后创建的并可能有权访问这些对象，其不最初目的。 
  
     若要说明，请考虑名为 Amy 简介中提到的新员工的示例。 因为它创建用于还原域在备份后，Amy 的用户对象不再存在还原操作之后。 但是，还原操作之后可能会保留已分配给该用户对象的任何访问权限。 如果该用户对象 SID 重新分配给一个新的对象在还原操作后，新的对象要获得这些访问权限。 
  
9. 使当前 RID 池失效。 当前 RID 池失效后系统状态还原。 但如果未执行系统状态还原，当前 RID 池需要可以在失效以免已还原的 DC 重新从已在创建备份时的时间分配 RID 池颁发 Rid。 有关详细信息，请参阅[使当前 RID 池失效](AD-Forest-Recovery-Invaildate-RID-Pool.md)。 
  
    > [!NOTE]
    > 第一次尝试使用一个 SID 创建对象，您使 RID 池失效后将收到错误。 尝试创建一个对象将触发新的 RID 池的请求。 重试该操作的成功，因为将分配给新的 RID 池。 
  
10. 两次重置此 DC 的计算机帐户密码。 有关详细信息，请参阅[的域控制器的计算机帐户密码重置](AD-Forest-Recovery-Reset-Computer-Account-DC.md)。 
  
11. 两次重置 krbtgt 密码。 有关详细信息，请参阅[重置 krbtgt 密码](AD-Forest-Recovery-Resetting-the-krbtgt-password.md)。 
  
     因为 krbtgt 密码历史记录，这两个密码重置密码两次以从密码历史记录中删除原始 （故障前） 密码。 
  
    > [!NOTE]
    > 如果林恢复对安全事件响应中，您可能还重置信任密码。 有关详细信息，请参阅[重置上一方信任的信任密码](AD-Forest-Recovery-Reset-Trust.md)。 
  
12. 如果林中有多个域，而还原的 DC 的全局编录服务器在故障发生前的清除**全局编录**中要从域控制器删除全局编录的 NTDS 设置属性的复选框。 此规则的例外是林的常见的情况下具有一个域。 在这种情况下，它不需要删除全局编录。 有关详细信息，请参阅[删除全局编录](AD-Forest-Recovery-Remove-GC.md)。 
  
     通过从更多的备份还原全局编录比其他用于还原其他域中的 Dc 的备份最近您可能会引入延迟对象。 请考虑下面的示例。 在域 A 中，通过在时间 T1 拍摄的备份还原 DC1。 在域 B 中，DC2 从在时间 T2 拍摄全局编录备份中还原。 假设晚于 T1，T2，T1 和 T2 之间创建了一些对象。 还原这些 Dc 后，DC2，这是全局编录，保留较新数据域 A 的部分副本比域 A 保存本身。 DC2，这种情况下，保留延迟对象，因为这些对象不是在 DC1 上存在。 
  
     延迟对象存在可能导致问题。 例如，不可能将电子邮件消息传递到其用户对象已在域之间迁移的用户。 将过时的 DC 或全局编录服务器重新联机后，用户对象的两个实例出现在全局编录。 这两个对象具有相同的电子邮件地址;因此，不能将电子邮件消息传递。 
  
     第二个问题是，不再存在的用户帐户可能仍会显示在全局地址列表中。 第三个问题是，不再存在一个通用组可能仍会显示在用户的访问令牌。 
  
     如果未还原的 DC 是全局编录，— 是无意中或因为这是你信任的独立备份，我们建议您禁止通过该还原操作后很快禁用全局编录延迟对象的匹配项完成。 禁用全局编录标志将导致丢失所有其部分副本 （分区） 和正则 DC 状态 relegating 本身的计算机。 
  
13. 配置 Windows 时间服务。 在林根域中，将 PDC 模拟器配置为从外部时间源同步时间。 有关详细信息，请参阅[中目录林根域的 PDC 仿真器上配置 Windows 时间服务](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29)。 
  
## <a name="reconnect-each-restored-writeable-domain-controller-to-a-common-network"></a>重新连接到公共网络的每个还原的可写域控制器

在此阶段应在目录林根域中并在每个剩余的域具有还原一个 DC （和执行的恢复步骤）。 将这些域控制器加入到环境的其余部分隔离的常见网络并完成以下步骤才能验证林的运行状况和复制。

> [!NOTE]
> 当物理域控制器加入到隔离的网络时，可能需要更改其 IP 地址。 因此，DNS 记录的 IP 地址会不正确。 全局编录服务器不可用，因为 dns 安全动态更新将失败。 在这种情况下由于它们可以为新的虚拟网络联接，而无需更改其 IP 地址，因此虚拟域控制器是更为有利。 这是为何虚拟 Dc 我们建议使用第一个域控制器作为林恢复过程中要还原的原因之一。 
  
验证后，将在域控制器加入到生产网络并完成验证林复制运行状况的步骤。

- 若要修复名称解析，创建 DNS 委派记录，并配置 DNS 转发和根提示根据需要。 运行**repadmin /replsum**检查 Dc 之间进行复制。 
- 如果已还原的 DC 不是直接复制伙伴，复制恢复会快得多通过创建它们之间的临时连接对象。 
- 若要验证元数据清除，请运行**Repadmin /viewlist \\** * 有关在林中所有域控制器的列表。 运行**Nltest /DCList:** *< 域\>* 有关域中的所有域控制器的列表。 
- 若要检查 DC 和 DNS 的运行状况，请在林中所有域控制器上运行报告错误 DCDiag /v。 

## <a name="add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain"></a>将全局编录添加到林根级域中的域控制器

全局编录是这些和其他原因所必需的：  
  
- 若要启用的用户登录。 
- 若要启用运行中的每个子域的域控制器，以注册和根域中删除 DNS 服务器上的记录上的 Net Logon 服务。 
  
虽然它是首选的林根 DC 成为全局编录，则可以选择任何已还原域控制器成为全局编录。 
  
> [!NOTE]
> 将为全局编录服务器播发 DC，直到它完成在林中的所有目录分区的完全同步。 因此，应强制 DC 复制与每个林中的已还原域控制器。 
>
> 监视目录服务事件日志事件查看器中的事件 ID 1119，这表示此 DC 是全局编录服务器，或验证以下注册表项的值为 1:  
>
> **HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Global Catalog Promotion Complete**  
  
有关详细信息，请参阅[添加全局编录](AD-Forest-Recovery-Add-GC.md)。 
  
在此阶段应在林中具有稳定的林，与一个 DC 的每个域和一个全局编录。 应进行新备份的每个您刚刚还原的 Dc。 你现在可以开始重新安装 AD DS 部署在林中其他域控制器。 

## <a name="next-steps"></a>后续步骤

- [AD 林恢复 - 先决条件](AD-Forest-Recovery-Prerequisties.md)  
- [AD 林恢复-设计出一个自定义林恢复计划](AD-Forest-Recovery-Devising-a-Plan.md)  
- [AD 林恢复-识别问题](AD-Forest-Recovery-Identify-the-Problem.md)
- [AD 林恢复-确定如何恢复](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [AD 林恢复-执行初始恢复](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [AD 林恢复 - 过程](AD-Forest-Recovery-Procedures.md)  
- [AD 林恢复-方面的常见问题](AD-Forest-Recovery-FAQ.md)  
- [AD 林恢复-恢复单个的多域林中域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [AD 林恢复-与 Windows Server 2003 域控制器的林恢复](AD-Forest-Recovery-Windows-Server-2003.md)  
