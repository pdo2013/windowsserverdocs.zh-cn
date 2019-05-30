---
title: 安全地虚拟化的 Active Directory 域服务 (AD DS)
description: USN 回滚，Active Directory 的安全虚拟化
ms.topic: article
ms.prod: windows-server-threshold
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 03/22/2019
ms.technology: identity-adds
ms.assetid: 7a3114c8-bda8-49bb-83a8-4e04340ab221
ms.openlocfilehash: aa84e09e8a958193fee82c7b9c03cd1dca910c55
ms.sourcegitcommit: 2977c707a299929c6ab0d1e0adab2e1c644b8306
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2019
ms.locfileid: "63684188"
---
# <a name="safely-virtualizing-active-directory-domain-services-ad-ds"></a>安全地虚拟化的 Active Directory 域服务 (AD DS)

>适用于：Windows Server

从 Windows Server 2012 开始，AD DS 提供了更多虚拟化域控制器，通过引入虚拟化安全功能的支持。 本文介绍在域控制器复制的 Usn 和 InvocationIDs 角色，并讨论了一些可能出现的潜在问题。

## <a name="update-sequence-number-and-invocationid"></a>更新序列号和 InvocationID

虚拟环境对取决于基于逻辑时钟的复制方案的分布式工作负荷，提出了独特的挑战。 例如，AD DS 复制使用分配给每个域控制器上的事务的单调递增的值（称为 USN 或更新序列号）。 每个域控制器的数据库实例还提供一个标识，称为 InvocationID。 域控制器的 InvocationID 和其 USN 一起作为一个唯一标识符，该标识符与在每个域控制器上执行的每个写入事务相关联，并且在林中必须是唯一的。

AD DS 复制使用每个域控制器上的 InvocationID 和 USN 来确定需要复制到其他域控制器的更改。 如果域控制器域控制器感知之外及时回滚，并且 USN 重复用于完全不同的事务，因为其他域控制器会认为他们已经收到了更新，复制将无法聚合与该 InvocationID 的上下文下重复使用的 USN 相关联。

例如，下图显示了当在 VDC2（在虚拟机上运行的目标域控制器）上检测到 USN 回滚时在 Windows Server 2008 R2 和更早版本的操作系统上所发生事件的顺序。 在此图中，USN 回滚的检测时，发生 VDC2 上复制伙伴检测到 VDC2 已发送了以前看到的复制伙伴，这表示，VDC2 的数据库已及时回滚最新 USN 值不正确。

![检测到 USN 回滚时的事件序列](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_Exampleofhowreplicationcanbecomeinconsistent.png)

虚拟机 (VM) 可以轻松为虚拟机监控程序管理员回滚域控制器的 Usn （其逻辑时钟），例如，应用于域控制器感知之外的快照。 有关 USN 和 USN 回滚的详细信息，包括演示未检测到的 USN 回滚实例的另一个图，请参阅 [USN 和 USN 回滚](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx#usn_and_usn_rollback)。

从 Windows Server 2012 开始，将公开名为 VM 生成 ID 的标识符的虚拟机监控程序平台上托管的 AD DS 虚拟域控制器可以检测，并采取必要的安全措施保护 AD DS 环境，如果虚拟机将会回滚返回在应用程序的虚拟机快照的时间。 因为虚拟机生成 ID 设计使用虚拟机监控程序供应商独立机制在来宾虚拟机地址空间中显示此标识符，所以安全虚拟化体验对支持虚拟机生成 ID 的任何虚拟机监控程序来说始终可用。 如果虚拟机已及时回滚，则此标识符可以通过在虚拟机内运行的服务和应用程序进行采样加以检测。

## <a name="effects-of-usn-rollback"></a>USN 回滚的影响

当发生了 USN 回滚时，对对象和属性不修改入站复制之前查看过 USN 的目标域控制器。

因为这些目标域控制器认为它们是最新，在目录服务事件日志中或通过监视和诊断工具未不报告任何复制错误。

USN 回退可能会影响任何对象或任何分区中的属性的复制。 最常观察到的副作用是用户帐户和在回退域控制器创建的计算机帐户不存在的一个或多个复制伙伴上。 或者，源自回退域控制器的密码更新复制合作伙伴上不存在。

USN 回滚可以防止任何 Active Directory 分区中的任何对象类型复制。 这些对象类型包括：

* Active Directory 复制拓扑和计划
* 在这些域控制器持有的角色以及在林中的域控制器存在
* 林中的域和应用程序分区存在
* 安全组和当前组成员之后存在
* 在 Active Directory 集成 DNS 区域中的 DNS 记录注册

USN 孔的大小可能会给用户、 计算机、 信任、 密码和安全组表示数百、 数千个或甚至数万个更改。 USN 空洞的定义之间的最高 USN 号时进行还原的系统状态备份和原始更改的数量存在已脱机之前回滚域控制器上创建的差异。

## <a name="detecting-a-usn-rollback"></a>检测 USN 回滚

因为很难检测到 USN 回滚，域控制器将记录事件 2095 时源域控制器将以前已确认的 USN 号发送到目标域控制器，而无需相应更改调用 id。

若要防止唯一源自更新到 Active Directory 上未正确还原的域控制器中，创建的 Net Logon 服务已暂停。 Net Logon 服务暂停时，用户和计算机帐户不能更改将不出站复制此类更改的域控制器上的密码。 同样，Active Directory 管理工具更倾向于正常的域控制器时它们对 Active Directory 中的对象进行更新。

在域控制器上，如下所示的事件消息记录中如果满足以下条件：

* 源域控制器将大量以前确认的 USN 发送到目标域控制器。
* 没有任何相应的更改在调用 id。

可能会在目录服务事件日志中捕获这些事件。 但是，它们可能会覆盖之前管理员看到它们。

如果您怀疑 USN 回滚已出现但看不到相应的事件在事件日志，检查 DSA 不可写的注册表项中。 此条目提供法庭证据发生 USN 回滚。

```
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\NTDS\Parameters
Registry entry: Dsa Not Writable
Value: 0x4
```

> [!WARNING]
> 删除或手动更改 Dsa 不可写的注册表条目值置于永久不受支持状态回退域控制器。 因此，不支持此类更改。 具体而言，修改值中删除由 USN 回滚检测代码添加的隔离行为。 回退域控制器上的 Active Directory 分区将永久与同一个 Active Directory 林中的指导支持和可传递复制伙伴不一致。

此注册表项和解决方法步骤的详细信息可在支持文章[Active Directory 复制错误 8456 或 8457:"源 |目标服务器当前拒绝复制请求"](https://support.microsoft.com/help/2023007/active-directory-replication-error-8456-or-8457-the-source-destination)。

## <a name="virtualization-based-safeguards"></a>基于虚拟化安全措施

域控制器安装期间，AD DS 最初将存储的 VM 生成 Id 标识符作为在其数据库 （通常称为目录信息树或 DIT） 中的域控制器的计算机对象上的 msDS-GenerationID 属性的一部分。 虚拟机内的 Windows 驱动程序独立地跟踪虚拟机生成 ID。

当管理员从以前的快照还原虚拟机时，来自虚拟机驱动程序的虚拟机生成 ID 的当前值要与 DIT 中的值进行比较。

如果这两个值不相同，则重置 invocationID 并放弃 RID 池，从而防止重复使用 USN。 如果这两个值相同，则正常提交事务。

每次域控制器重新启动时，AD DS 也将来自虚拟机的虚拟机生成 ID 的当前值与 DIT 中的值进行比较，如果这两个值不相同，则它将重置 invocationID，放弃 RID 池，并使用新值更新 DIT。 它也非权威地同步 SYSVOL 文件夹，以完成安全还原。 这使安全措施扩展到关闭的虚拟机上快照的应用程序。 在 Windows Server 2012 中引入的这些安全措施使 AD DS 管理员能够从部署和管理虚拟化环境中的域控制器的独特优势中受益。

下图显示在支持 VM 生成 Id 的虚拟机监控程序运行 Windows Server 2012 的虚拟化的域控制器上检测到相同的 USN 回滚时如何应用虚拟化安全措施。

![检测到相同的 USN 回滚时应用的安全措施](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_VDC_Exampleofhowsafeguardswork.gif)

在这种情况下，当虚拟机监控程序检测到虚拟机生成 ID 值更改时，将触发虚拟化安全措施，包括重置虚拟化 DC 的 InvocationID（在前面的示例中从 A 到 B），更新保存在虚拟机上的虚拟机生成 ID 值以匹配虚拟机监控程序存储的新值 (G2)。 安全措施确保两个域控制器的复制聚合。

与 Windows Server 2012 AD DS 使用托管在 VM 生成 Id 感知的虚拟机监控程序上的虚拟域控制器上的安全措施，并确保意外的快照或其他此类虚拟机监控程序启用的机制，无法回滚的虚拟应用程序计算机的状态 （通过防止 USN 泡沫等复制问题或延迟对象） 不会中断 AD DS 环境。

作为一种替代机制备份域控制器，不建议通过应用虚拟机快照还原域控制器。 建议你继续使用 Windows Server Backup 或其他基于 VSS 书写器的备份解决方案。

> [!CAUTION]
> 如果在生产环境中的域控制器意外地还原到快照，则建议您向供应商咨询有关验证的状态后这些程序的指南，该虚拟机上承载的应用程序及服务快照还原。

有关详细信息，请参阅[虚拟化域控制器安全还原体系结构](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch)。

## <a name="recovering-from-a-usn-rollback"></a>从 USN 回滚中恢复

有两种方法从 USN 回滚中恢复：

* 从域中删除域控制器
* 还原一个好的备份系统的状态

### <a name="remove-the-domain-controller-from-the-domain"></a>从域中删除域控制器

1. 从域控制器，以强制其成为独立的服务器中删除 Active Directory。
2. 关闭已降级的服务器。
3. 在正常运行的域控制器上，清除已降级的域控制器的元数据。
4. 如果未正确还原的域控制器托管操作主机角色，请将这些角色转移到正常的域控制器。
5. 重启已降级的服务器。
6. 如果所需 Active Directory 在独立服务器上重新安装。
7. 如果域控制器以前是全局编录，则配置域控制器成为全局编录。
8. 如果域控制器以前托管操作主机角色，转移的操作主机角色重新到域控制器。

### <a name="restore-the-system-state-of-a-good-backup"></a>还原一个好的备份系统的状态

评估是否此域控制器存在有效的系统状态备份。 如果回退的域控制器错误还原，且备份中包含最新的域控制器所做的更改之前，已有效的系统状态备份，请从最新备份还原系统状态。

此外可以作为备份的源使用快照。 您可以将数据库设置为向自己提供新的部分中使用该过程的调用 ID 或[相应的系统状态数据备份不可用时还原的虚拟域控制器](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd363553%28v%3dws.10%29#restoring-a-virtual-domain-controller-when-an-appropriate-system-state-data-backup-is-not-available)

## <a name="next-steps"></a>后续步骤

* 有关虚拟域控制器的更多疑难解答信息，请参阅[虚拟域控制器疑难解答](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md)。
* [有关 Windows 时间服务 (W32Time) 的详细的信息](../../networking/windows-time-service/windows-time-service-top.md)
