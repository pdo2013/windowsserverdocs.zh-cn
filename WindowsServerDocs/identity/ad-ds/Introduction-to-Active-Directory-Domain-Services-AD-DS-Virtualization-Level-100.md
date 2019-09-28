---
title: 安全虚拟化 Active Directory 域服务（AD DS）
description: Active Directory 的 USN 回滚和安全虚拟化
ms.topic: article
ms.prod: windows-server
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 03/22/2019
ms.technology: identity-adds
ms.assetid: 7a3114c8-bda8-49bb-83a8-4e04340ab221
ms.openlocfilehash: 67e35a47467b1f5f66bfd073c6f9db06094ea3f9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71391031"
---
# <a name="safely-virtualizing-active-directory-domain-services-ad-ds"></a>安全虚拟化 Active Directory 域服务（AD DS）

>适用于：Windows Server

从 Windows Server 2012 开始，通过引入虚拟化安全功能，AD DS 为虚拟化域控制器提供了更大的支持。 本文介绍了域控制器复制中的 Usn 和 InvocationIDs 的角色，并讨论了可能出现的一些潜在问题。

## <a name="update-sequence-number-and-invocationid"></a>更新序列号和 InvocationID

虚拟环境对取决于基于逻辑时钟的复制方案的分布式工作负荷，提出了独特的挑战。 例如，AD DS 复制使用分配给每个域控制器上的事务的单调递增的值（称为 USN 或更新序列号）。 还为每个域控制器的数据库实例提供一个标识，称为 InvocationID。 域控制器的 InvocationID 和其 USN 一起作为一个唯一标识符，该标识符与在每个域控制器上执行的每个写入事务相关联，并且在林中必须是唯一的。

AD DS 复制使用每个域控制器上的 InvocationID 和 USN 来确定需要复制到其他域控制器的更改。 如果域控制器在域控制器感知之外及时回滚，并且 USN 可用于完全不同的事务，则复制将无法聚合，因为其他域控制器会认为他们已经收到更新与 InvocationID 的上下文中重新使用的 USN 相关联。

例如，下图显示了当在 VDC2（在虚拟机上运行的目标域控制器）上检测到 USN 回滚时在 Windows Server 2008 R2 和更早版本的操作系统上所发生事件的顺序。 在此图中，当复制伙伴检测到 VDC2 已发送最新 USN 值（以前由复制伙伴查看）时，VDC2 上将进行 USN 回滚的检测，这表示 Vdc2 数据库已及时回滚非.

![检测到 USN 回滚时的事件序列](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_Exampleofhowreplicationcanbecomeinconsistent.png)

例如，使用虚拟机（VM），虚拟机监控程序管理员可以轻松地回滚域控制器的 Usn （其逻辑时钟），例如，在域控制器的感知范围之外应用快照。 有关 USN 和 USN 回滚的详细信息，包括演示未检测到的 USN 回滚实例的另一个图，请参阅 [USN 和 USN 回滚](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx#usn_and_usn_rollback)。

从 Windows Server 2012 开始，在虚拟机监控程序平台上托管的虚拟域控制器 AD DS 公开名为 VM 生成 ID 的标识符，则可以检测和采取必要的安全措施来保护 AD DS 环境（如果虚拟机已被滚动）VM 快照的应用程序。 因为虚拟机生成 ID 设计使用虚拟机监控程序供应商独立机制在来宾虚拟机地址空间中显示此标识符，所以安全虚拟化体验对支持虚拟机生成 ID 的任何虚拟机监控程序来说始终可用。 如果虚拟机已及时回滚，则此标识符可以通过在虚拟机内运行的服务和应用程序进行采样加以检测。

## <a name="effects-of-usn-rollback"></a>USN 回滚的影响

发生 USN 回滚时，对对象和属性所做的修改不会被目标域控制器的入站复制，此域控制器之前已显示了 USN。

由于这些目标域控制器认为它们是最新的，因此在目录服务事件日志中或通过监视和诊断工具不会报告复制错误。

USN 回滚可能会影响任何分区中任何对象或属性的复制。 观察到的最常见的副作用是，在回滚域控制器上创建的用户帐户和计算机帐户在一个或多个复制伙伴上不存在。 或者，源自回退域控制器的密码更新在复制伙伴上不存在。

USN 回滚可防止任何 Active Directory 分区中的任何对象类型进行复制。 这些对象类型包括：

* Active Directory 复制拓扑和计划
* 林中是否存在域控制器以及这些域控制器持有的角色
* 林中是否存在域和应用程序分区
* 存在安全组及其当前组成员身份
* Active Directory 集成 DNS 区域中的 DNS 记录注册

USN 洞的大小可能表示对用户、计算机、信任、密码和安全组的数百、上千甚至数十个更改。 USN 洞由在已还原的系统状态备份时存在的最高 USN 数与在脱机之前在回滚的域控制器上创建的原始更改数进行定义。

## <a name="detecting-a-usn-rollback"></a>检测 USN 回滚

由于 USN 回滚很难检测到，因此，当源域控制器向目标域控制器发送以前确认的 USN 号，而在调用 ID 中没有相应的更改时，域控制器会记录事件2095。

为了防止在不正确还原的域控制器上创建 Active Directory 的唯一原始更新，Net Logon 服务已暂停。 当 Net Logon 服务暂停时，用户和计算机帐户无法更改不会出站复制此类更改的域控制器上的密码。 同样，在对 Active Directory 中的对象进行更新时，Active Directory 管理工具将优先于正常的域控制器。

在域控制器上，如果满足以下条件，则会记录类似于以下内容的事件消息：

* 源域控制器向目标域控制器发送以前确认的 USN 号。
* 调用 ID 中没有相应的更改。

可以在目录服务事件日志中捕获这些事件。 但是，它们可能会在管理员观察到它们之前被覆盖。

如果你怀疑 USN 回滚已发生，但未在事件日志中看到对应的事件，请检查注册表中的 "不能写入 DSA" 条目。 此条目提供了 USN 回滚的法庭证据。

```
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\NTDS\Parameters
Registry entry: Dsa Not Writable
Value: 0x4
```

> [!WARNING]
> 删除或手动更改 "不能写入的 Dsa 注册表项" 值会使回滚域控制器处于永久性不受支持的状态。 因此，不支持此类更改。 具体而言，修改该值将删除 USN 回滚检测代码添加的隔离行为。 回滚域控制器上的 Active Directory 分区将与同一 Active Directory 林中的直接和可传递复制伙伴永久不一致。

有关此注册表项和解决方法的详细信息，请参阅支持文章 [Active Directory 复制错误8456或8457："源 |目标服务器当前拒绝复制请求 "](https://support.microsoft.com/help/2023007/active-directory-replication-error-8456-or-8457-the-source-destination)。

## <a name="virtualization-based-safeguards"></a>基于虚拟化的安全措施

安装域控制器期间，最初 AD DS 会将 VM 生成 id 标识符作为其数据库（通常称为目录信息树，或 DIT）中域控制器计算机对象上的生成 id 属性的一部分进行存储。 虚拟机内的 Windows 驱动程序独立地跟踪虚拟机生成 ID。

当管理员从以前的快照还原虚拟机时，来自虚拟机驱动程序的虚拟机生成 ID 的当前值要与 DIT 中的值进行比较。

如果这两个值不相同，则重置 invocationID 并放弃 RID 池，从而防止重复使用 USN。 如果这两个值相同，则正常提交事务。

每次域控制器重新启动时，AD DS 也将来自虚拟机的虚拟机生成 ID 的当前值与 DIT 中的值进行比较，如果这两个值不相同，则它将重置 invocationID，放弃 RID 池，并使用新值更新 DIT。 它也非权威地同步 SYSVOL 文件夹，以完成安全还原。 这使安全措施扩展到关闭的虚拟机上快照的应用程序。 Windows Server 2012 中引入的这些安全措施使 AD DS 管理员可以受益于在虚拟化环境中部署和管理域控制器的独特优势。

下图显示了在支持 VM 生成 id 的虚拟机监控程序上运行 Windows Server 2012 的虚拟化域控制器上检测到相同的 USN 回滚时，如何应用虚拟化安全措施。

![检测到相同的 USN 回滚时应用的安全措施](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_VDC_Exampleofhowsafeguardswork.gif)

在这种情况下，当虚拟机监控程序检测到虚拟机生成 ID 值更改时，将触发虚拟化安全措施，包括重置虚拟化 DC 的 InvocationID（在前面的示例中从 A 到 B），更新保存在虚拟机上的虚拟机生成 ID 值以匹配虚拟机监控程序存储的新值 (G2)。 安全措施确保两个域控制器的复制聚合。

使用 Windows Server 2012，AD DS 在托管于 VM 生成 id 的虚拟机监控程序上使用虚拟域控制器上的安全措施，并确保意外应用了快照或其他此类启用了虚拟机监控程序的机制来回滚虚拟计算机的状态不会中断 AD DS 环境（通过防止 USN 冒泡或延迟对象等复制问题）。

不建议通过应用虚拟机快照还原域控制器作为备份域控制器的备用机制。 建议你继续使用 Windows Server Backup 或其他基于 VSS 书写器的备份解决方案。

> [!CAUTION]
> 如果生产环境中的域控制器意外地还原到快照，则建议你向供应商咨询应用程序、托管在该虚拟机上的服务、快照还原。

有关详细信息，请参阅[虚拟化域控制器安全还原体系结构](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch)。

## <a name="recovering-from-a-usn-rollback"></a>从 USN 回滚恢复

可以通过两种方法从 USN 回滚恢复：

* 从域中删除域控制器
* 还原正常备份的系统状态

### <a name="remove-the-domain-controller-from-the-domain"></a>从域中删除域控制器

1. 从域控制器中删除 Active Directory，以将其强制成为独立服务器。
2. 关闭已降级的服务器。
3. 在运行状况良好的域控制器上，清除降级的域控制器的元数据。
4. 如果错误还原的域控制器承载操作主机角色，请将这些角色传输到正常的域控制器。
5. 重新启动已降级的服务器。
6. 如果需要，请在独立服务器上再次安装 Active Directory。
7. 如果域控制器以前是全局编录，请将域控制器配置为全局编录。
8. 如果域控制器以前托管了操作主机角色，请将操作主机角色转移回域控制器。

### <a name="restore-the-system-state-of-a-good-backup"></a>还原正常备份的系统状态

评估此域控制器是否存在有效的系统状态备份。 如果在不正确还原回滚域控制器之前创建了有效的系统状态备份，并且备份包含域控制器上所做的最新更改，请从最新备份还原系统状态。

你还可以使用快照作为备份源。 或者，你可以将数据库设置为具有一个新的调用 ID，方法是使用在[适当的系统状态数据备份不可用时还原虚拟域控制器](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd363553%28v%3dws.10%29#restoring-a-virtual-domain-controller-when-an-appropriate-system-state-data-backup-is-not-available)部分中的过程

## <a name="next-steps"></a>后续步骤

* 有关虚拟域控制器的更多疑难解答信息，请参阅[虚拟域控制器疑难解答](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md)。
* [有关 Windows 时间服务（W32Time）的详细信息](../../networking/windows-time-service/windows-time-service-top.md)
