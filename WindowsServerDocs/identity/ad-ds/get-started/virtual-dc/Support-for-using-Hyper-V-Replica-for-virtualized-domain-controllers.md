---
ms.assetid: 45a65504-70b5-46ea-b2e0-db45263fabaa
title: 支持将 Hyper-V 副本用于虚拟化域控制器
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 92324ef7c0fab81e80974a1f05eeec4833f09875
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390428"
---
# <a name="support-for-using-hyper-v-replica-for-virtualized-domain-controllers"></a>支持将 Hyper-V 副本用于虚拟化域控制器

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主题介绍了对于使用 Hyper-V 副本复制作为域控制器 (DC) 运行的虚拟机 (VM) 的支持能力。 Hyper-V 副本是始于 Windows Server 2012 的 Hyper-V 新功能，它提供虚拟机级别的内置复制机制。  
  
Hyper-V 副本通过 LAN 或 WAN 链接以异步方式将所选虚拟机从主 Hyper-V 主机复制到副本 Hyper-V 主机。 完成初始复制后，将按照管理员定义的时间间隔复制后续的更改。  
  
故障转移可以是计划的或非计划的。 计划的故障转移由管理员在主虚拟机上启动，它将任何未复制的更改复制到副本虚拟机上，以防止丢失任何数据。 将在副本虚拟机上启动非计划的故障转移，以响应主虚拟机中的意外失败。 因为没有机会传输可能尚未复制的主虚拟机上的更改，所以可能会丢失数据。  
  
有关 Hyper-V 副本的详细信息，请参阅 [Hyper-V 副本概述](https://technet.microsoft.com/library/jj134172.aspx) 和 [部署 Hyper-V 副本](https://technet.microsoft.com/library/jj134207.aspx)。  
  
> [!NOTE]  
> 仅可在 Windows Server Hyper-V（不是在 Windows 8 上运行的 Hyper-V 版本）上运行 Hyper-V 副本。  
  
## <a name="windows-server-2012-or-newer-domain-controllers-required"></a>需要 Windows Server 2012 或更高版本的域控制器

Windows Server 2012 Hyper-v 引入了 VM-生成 id （VMGenID）。 VMGenID 提供了一种方法，可在发生重大更改时，使虚拟机监控程序与来宾 OS 进行通信。 例如，虚拟机监控程序可以与已从快照还原（Hyper-V 快照还原技术，而不是备份还原）的虚拟化 DC 进行通信。 Windows Server 2012 和更高版本中的 AD DS 可感知 VMGenID VM 技术，并使用它来检测何时执行虚拟机监控程序操作（例如快照还原），从而允许它更好地保护自身。  
  
> [!NOTE]
> 只有 Windows Server 2012 Dc 或更高版本上的 AD DS 提供从 VMGenID 生成的这些安全措施;运行所有以前版本的 Windows Server 的 Dc 受在使用不受支持的机制（例如快照还原）还原虚拟化 DC 时可能发生的问题。 有关这些安全措施和何时触发它们的详细信息，请参阅 [虚拟化域控制器体系结构](https://technet.microsoft.com/library/jj574118.aspx)。  
  
当 Hyper-v 副本故障转移发生（计划内或计划外）时，虚拟化的 DC 会检测到 VMGenID 重置，并触发上述安全功能。 然后，Active Directory 操作将如常地继续。 副本 VM 取代主 VM 运行。  
  
> [!NOTE]  
> 考虑到现在存在两个具有相同 DC 标识的实例，有可能主要实例和复制的实例都将运行。 尽管 Hyper-V 副本已准备控制机制以确保主 VM 和副本 VM 不会同时运行，但如果发生它们之间的链接在虚拟机复制之后失败的事件，它们仍可能会同时运行。 在这个不太可能发生的事件中，运行 Windows Server 2012 的虚拟化 DC 具有有助于保护 AD DS 的安全措施，而运行更早版本 Windows Server 的虚拟化 DC 没有这些安全措施。  
  
使用 Hyper-V 副本时，请确保按照 [在 Hyper-V 上运行虚拟域控制器](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(v=WS.10).aspx)的最佳实践进行操作。 例如，这部分讨论了关于在虚拟 SCSI 磁盘上存储 Active Directory 文件的建议，从而对数据持久性提供了更强有力的保证。  
  
## <a name="supported-and-unsupported-scenarios"></a>支持和不支持的方案

非计划的故障转移和测试故障转移仅支持运行 Windows Server 2012 或更高版本的 Vm。 即使对于计划的故障转移，也建议为虚拟化 DC 使用 Windows Server 2012 或更高版本，以便在管理员不小心同时启动主 VM 和复制 VM 的事件中降低风险。  
  
计划的故障转移支持运行更早版本 Windows Server 的 VM，但因为有 USN 回滚的可能，所以未计划的故障转移不支持运行这类 VM。 有关 USN 回滚的详细信息，请参阅 [USN 和 USN 回滚](https://technet.microsoft.com/library/d2cae85b-41ac-497f-8cd1-5fbaa6740ffe(v=ws.10))。  
  
> [!NOTE]  
> 对于域或林没有任何功能级别的要求；仅对作为 VM（使用 Hyper-V 副本复制）运行的 DC 存在操作系统要求。 可以在包含其他物理或虚拟 DC 的林中部署 VM，这些 DC 运行较早版本的 Windows Server，并且不一定使用 Hyper-V 副本进行复制。  
  
此支持声明基于在单个域林中执行的测试，但是也支持多域林配置。 对于这些测试，虚拟化域控制器 DC1 和 DC2 是同一站点中的 Active Directory 复制伙伴，它们托管于 Windows Server 2012 上运行 Hyper-V 的服务器。 运行 DC2 的 VM 来宾已启用 Hyper-V 副本。 由另一个在地理上相隔很远的数据中心托管副本服务器。 为了帮助说明如下所述的测试用例过程，将副本服务器上运行的 VM 称为 DC2-Rec（尽管在实践中，它将保留与原始 VM 相同的名称）。  
  
### <a name="windows-server-2012"></a>Windows Server 2012

下表介绍了对运行 Windows Server 2012 的虚拟化 DC 的支持和测试用例。  
  
|||  
|-|-|  
|计划的故障转移|非计划的故障转移|  
|支持|支持|  
|测试用例：<br /><br />-DC1 和 DC2 正在运行 Windows Server 2012。<br /><br />-DC2 已关闭，并在 DC2-Rec 上执行故障转移。故障转移可以为计划的或非计划的。<br /><br />-在 DC2 开始后，它将检查其数据库中的 VMGenID 值是否与 Hyper-v 副本服务器保存的虚拟机驱动程序中的值相同。<br /><br />-因此，DC2 记录触发了虚拟化安全措施;换句话说，它会重置其 InvocationID，丢弃其 RID 池，并在其假定为操作主机角色之前设置初始同步要求。 有关初始同步要求的详细信息，请参阅 。<br /><br />-DC2 将 VMGenID 的新值保存在其数据库中，并在新 InvocationID 的上下文中提交任何后续更新。<br /><br />-由于 InvocationID 重置，DC1 将聚合到 DC2-Rec 引入的所有 AD 更改上，即使它是及时回滚的，这意味着在故障转移后，在 DC2 记录上执行的任何 AD 更新将安全地聚合起来|对于计划的故障转移，测试用例相同，但存在下列例外：<br /><br />-在发生故障转移事件之前，在 DC2 上收到但尚未由 AD 复制到复制伙伴的 AD 更新将丢失。<br /><br />-在由 AD 向 DC1 复制的恢复点之后，在 DC2 上收到的 AD 更新将从 DC1 复制回 DC2-Rec。|  
  
### <a name="windows-server-2008-r2-and-earlier-versions"></a>Windows Server 2008 R2 及较早版本

下表介绍了对运行 Windows Server 2008 R2 及较早版本的虚拟化 DC 的支持。  
  
|||  
|-|-|  
|计划的故障转移|非计划的故障转移|  
|支持但不推荐，因为运行这些版本的 Windows Server 的 Dc 不支持 VMGenID 或使用关联的虚拟化安全措施。 这使它们面临 USN 回滚的风险。 有关详细信息，请参阅 [USN 和 USN 回滚](https://technet.microsoft.com/library/d2cae85b-41ac-497f-8cd1-5fbaa6740ffe(v=ws.10))。|不支持**注意：** 不存在 USN 回滚风险时将支持非计划的故障转移，例如林中的单个域控制器（不推荐的配置）。|  
|测试用例：<br /><br />-DC1 和 DC2 正在运行 Windows Server 2008 R2。<br /><br />-DC2 已关闭，并在 DC2-Rec 上执行计划的故障转移。完成关闭操作之前，将 DC2 上的所有数据都复制到 DC2-Rec。<br /><br />-在 DC2 开始后，它会使用与 DC2 相同的 invocationID 恢复与 DC1 的复制。|不可用|  
