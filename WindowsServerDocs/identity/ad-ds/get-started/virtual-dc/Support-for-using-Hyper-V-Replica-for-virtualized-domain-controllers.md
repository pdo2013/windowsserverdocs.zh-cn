---
ms.assetid: 45a65504-70b5-46ea-b2e0-db45263fabaa
title: "对使用适用于虚拟化的域控制器 HYPER-V 副本的支持"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0444198196ed08a22aba92a0f59cc6e7a2518a2e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="support-for-using-hyper-v-replica-for-virtualized-domain-controllers"></a>对使用适用于虚拟化的域控制器 HYPER-V 副本的支持

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本主题介绍使用 Hyper-V 副本复制为域控制器 (DC) 运行虚拟机 (VM) 的支持。 Hyper-V 副本是以提供了内置的复制机制，在 VM 级别的 Windows Server 2012 Hyper-V 开头的一项新功能。  
  
Hyper-V 副本异步会从复制 Hyper-V 主机到主 Hyper-V 主机的所选虚拟机的功能在 LAN 或 wan 复制。 完成初始复制后，后续更改复制由管理员的时间间隔。  
  
故障转移可以计划或未计划。 计划的故障转移由管理员主要 VM 中，启动，并且未复制的任何更改轻复制到副本 VM 防止数据丢失。 计划的故障转移副本 VM 响应主要 VM 意外故障上启动。 因为没有机会传输上可能不尚未复制主要 VM 更改可能会丢失数据。  
  
有关 Hyper-V 副本的详细信息，请参阅[Hyper-V 副本概述](https://technet.microsoft.com/library/jj134172.aspx)和[部署 Hyper-V 副本](https://technet.microsoft.com/library/jj134207.aspx)。  
  
> [!NOTE]  
> Hyper-V 仅在 Windows Server Hyper-V，不 Hyper-V 在 Windows 8 上运行的是版本上，可以运行副本。  
  
## <a name="windows-server-2012-domain-controllers-required"></a>Windows Server 2012 所需的域控制器  
Windows Server 2012Hyper-V 还引入了 VM-GenerationID (VMGenID)。 VMGenID 提供了虚拟机监控程序进行通信的来宾操作系统到时已发生的重大更改的方法。 例如，虚拟机监控程序可以传达给虚拟化直流（Hyper-V 快照还原技术，不是备份还原）发生恢复的快照。 在 Windows Server 2012 的广告 DS 已注意到 VMGenID VM 技术，并使用它来虚拟机监控程序的操作，如快照还原，这使其更好地保护自身检测到。  
  
> [!NOTE]  
> 若要强调的点，广告 DS 仅在 Windows Server 2012 域控制器上的还提供了这些安全措施导致从 VMGenID;运行 Windows Server 的所有以前版本的 Dc 均受如 USN 回退使用不受支持的机制，如快照还原还原虚拟化的 DC 时可能会出现的问题。 有关这些保护和触发时的详细信息，请参阅[虚拟化域控制器体系结构](https://technet.microsoft.com/library/jj574118.aspx)。  
  
当 Hyper-V 副本故障转移时发生（计划或未计划）时，Windows Server 2012 虚拟化 DC 检测到 VMGenID 重置，从而触发上述安全功能。 然后 Active Directory 操作继续照常。 副本来主要 VM 替代虚拟机运行。  
  
> [!NOTE]  
> 现在现在有两个相同 DC 身份，可能会主要实例和复制的实例运行。 尽管 Hyper-V 副本以确保主有控制机制，可能无法同时运行副本虚拟机的功能，很可能他们在它们之间链接无法正常工作后的 VM 复制事件同时运行。 发生此可能不运行 Windows Server 2012 的虚拟化的 Dc 有保护以帮助保护广告 DS、虚拟化运行较早版本的 Windows Server 的 Dc 而未起效。  
  
在使用 Hyper-V 副本，确保您关注的最佳做法[运行虚拟域控制器上 Hyper-V](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(v=WS.10).aspx)。 这讨论，例如，建议用于存储上虚拟 SCSI 磁盘、Active Directory 文件的方式提供的数据持续性更强保障。  
  
## <a name="supported-and-unsupported-scenarios"></a>受支持不受支持的方案  
运行 Windows Server 2012 的 Vm 对计划的故障转移和测试故障转移受支持。 即使对计划的故障转移 Windows Server 2012 建议的虚拟化 DC 减少风险，在该管理员无意中启动主要 VM 和复制的 VM，同时为了。  
  
受支持的计划故障转移但由于 USN 回退可能不支持计划的故障转移的运行较早版本的 Windows Server 虚拟机的功能。 有关 USN 回退的详细信息，请参阅[USN 和 USN 回退](https://technet.microsoft.com/library/d2cae85b-41ac-497f-8cd1-5fbaa6740ffe(v=ws.10))。  
  
> [!NOTE]  
> 对于域或森林; 没有功能级别要求有仅操作系统的运行方式使用了 Hyper-V 副本复制虚拟机的功能 Dc 的要求。 可以包含其他物理或虚拟 Dc，运行较早版本的 Windows Server，可能需要也可能不还会复制使用了 Hyper-V 副本林中部署虚拟机的功能。  
  
此支持声明基于单个域-森林中, 执行的测试，但也支持多域森林配置。 这些测试，对于 DC1 和 DC2 虚拟化的域控制器是数据位于同一站点 Active Directory 复制合作伙伴运行 Hyper-V Windows Server 2012 的服务器上。 运行 DC2 VM 来宾已启用了 Hyper-V 副本。 遥远的其他数据中心中托管副本服务器。 为了帮助说明以下所述的测试用例进程，VM 副本服务器上运行称为 DC2 拍摄（但实际上，它将保留原始 VM 相同的名称）。  
  
### <a name="windows-server-2012"></a>Windows Server 2012  
下表介绍了有关运行 Windows Server 2012 和测试用例的虚拟化 Dc 的支持。  
  
|||  
|-|-|  
|计划故障转移|计划的故障转移|  
|受支持|受支持|  
|测试用例：<br /><br />-DC1 和 DC2 正在运行 Windows Server 2012。<br /><br />-DC2 则关闭并故障转移执行上 DC2 拍摄。在故障转移可以计划或未计划。<br /><br />-DC2 拍摄启动后，检查的值的 VMGenID 它与其数据库中具有是否值相同从通过 Hyper-V 副本服务器来保存虚拟机驱动程序。<br /><br />-这样一来，DC2 拍摄触发虚拟化保护;换言之，它将重置它 InvocationID，丢弃其 RID 池，并设置初始同步要求之前将假设操作主机角色。 有关初始同步要求的详细信息，请参阅。<br /><br />-然后，DC2 拍摄与其数据库中保存 VMGenID 新值，并将新 InvocationID 上下文中的任何更新后续提交。<br /><br />-作为的结果 InvocationID 重置，DC1 将集中全部广告更改引入的 DC2 拍摄，即使它已回滚的时间，即将安全地集中故障转移后 DC2 拍摄执行任何广告更新|测试用例已计划的故障转移，与这些异常一样：<br /><br />-任何广告更新接收上 DC2，但尚未由广告前复制到复制合作伙伴故障转移事件都将丢失。<br /><br />广告后已复制到 DC1 广告按恢复点时间将复制从 DC1 回 DC2 拍摄 DC2 上收到更新。|  
  
### <a name="windows-server-2008-r2-and-earlier-versions"></a>Windows Server 2008 R2 及更早版本  
下表介绍了有关运行 Windows Server 2008 R2 及更早版本的虚拟化 Dc 的支持。  
  
|||  
|-|-|  
|计划故障转移|计划的故障转移|  
|支持，但不是建议这样做，因为运行这些版本的 Windows Server 的 Dc 不支持 VMGenID 或使用的关联的虚拟化安全措施。 这将它们放在 USN 回退的风险。 有关详细信息，请参阅[USN 和 USN 回退](https://technet.microsoft.com/en-us/library/d2cae85b-41ac-497f-8cd1-5fbaa6740ffe(v=ws.10))。|不受支持**注意：**计划的故障转移想 USN 回退不存在的风险，如单个 DC 森林（不推荐配置）中受支持。|  
|测试用例：<br /><br />-DC1 和 DC2 正在运行 Windows Server 2008 R2。<br /><br />-DC2 则关闭并执行计划故障转移 DC2 拍摄。关闭并且在完成前，DC2 拍摄到复制 DC2 上的所有数据。<br /><br />-DC2 拍摄启动后，它将继续与 DC1 复制 DC2 为使用同一 invocationID。|不适用|  
  


