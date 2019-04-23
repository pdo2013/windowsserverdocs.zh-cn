---
ms.assetid: 6416d125-bcaf-433d-971a-2f0283bca2c2
title: 群集感知更新的方面的常见问题
ms.topic: article
ms.prod: windows-server-threshold
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 04/28/2017
description: 有关 Windows Server 中的群集感知更新常见问题的解答。
ms.openlocfilehash: f9009811093823554f16295cc1205f1b99ead93f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882518"
---
# <a name="cluster-aware-updating-frequently-asked-questions"></a>群集感知更新：常见问题

> 适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

[群集感知更新](cluster-aware-updating.md) \(CAU\)是一项功能，协调软件不会影响服务可用性的方式故障转移群集中的所有服务器上任何多个计划的故障转移群集节点的更新。 对于某些应用程序具有连续可用性功能\(如超\-V 实时迁移或 SMB 3.x 文件服务器具有 SMB 透明故障转移\)，CAU 能够协调自动化的群集更新而不会影响服务可用性。

## <a name="does-cau-support-updating-storage-spaces-direct-clusters"></a>CAU 是否支持更新的存储空间直通群集？  
是。 CAU 支持更新[存储空间直通](../storage/storage-spaces/storage-spaces-direct-overview.md)群集而不考虑部署类型： 超聚合或聚合。 具体而言，CAU 业务流程可确保，挂起每个群集节点等待基础的群集的存储空间，处于正常状态。

## <a name="does-cau-work-with-windowsserver-2008r2-or-windows7"></a>CAU 是否可以与 Windows Server 2008 R2 或 Windows 7 一起使用？  
否。 CAU 协调群集更新操作只能从运行 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows 10、 Windows 8.1 或 Windows 8 的计算机。 正在更新的故障转移群集必须运行 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012。
  
## <a name="is-cau-limited-to-specific-clustered-applications"></a>CAU 仅限于特定的群集应用程序？  
否。 CAU 对于群集应用程序类型是不可知的。 CAU 是外部群集\-更新之上群集 Api 和 PowerShell cmdlet 的解决方案。 在这种情况下，CAU 可以在 Windows Server 故障转移群集中配置任何群集应用程序更新进行协调。  
  
> [!NOTE]  
> 目前，以下群集工作负载测试和认证适用于 CAU:SMB、 超\-V、 DFS 复制、 DFS 命名空间、 iSCSI 和 NFS。  
  
## <a name="does-cau-support-updates-from-microsoft-update-and-windows-update"></a>CAU 是否支持从 Microsoft Update 和 Windows Update 更新？  
是。 默认情况下，CAU 配置有即插即用\-中，使用 Windows Update 代理\(WUA\)群集节点上的实用工具 Api。 WUA 基础结构可以配置为指向 Microsoft Update 和 Windows Update 或 Windows Server Update Services \(WSUS\)作为其更新源。  
  
## <a name="does-cau-support-wsus-updates"></a>CAU 是否支持 WSUS 更新？  
是。 默认情况下，CAU 配置有即插即用\-中，使用 Windows Update 代理\(WUA\)群集节点上的实用工具 Api。 WUA 基础结构可以配置为指向 Microsoft Update 和 Windows Update 或本地的 Windows Server Update Services \(WSUS\)服务器作为其更新源。  
  
## <a name="can-cau-apply-limited-distribution-release-updates"></a>CAU 是否可应用有限分发版本更新？  
是。 有限分发版本\(LDR\)更新，也称为修补程序，不发布通过 Microsoft 更新或 Windows 更新，因此它们不能下载 Windows 更新代理\(WUA\)插入\-，默认情况下使用 CAU。  
  
但是，CAU 包含第二个插件\-，可以选择要应用修补程序更新。 此修补程序插件\-中也可以自定义应用非\-Microsoft 驱动程序、 固件和 BIOS 更新。  
  
## <a name="can-i-use-cau-to-apply-cumulative-updates"></a>我是否可以使用 CAU 来应用累积更新？  
是。 如果累积更新是常规的分发版本更新或 LDR 更新，CAU 可以应用它们。  
  
## <a name="can-i-schedule-updates"></a>可以安排更新？  
是。 CAU 支持以下更新模式，都允许对更新进行计划：  
  
**自助\-更新**使群集能够自行更新根据定义的配置文件和常规日程，例如在每月的维护时段。 你还可以启动自\-更新运行按需在任何时间。 若要启用自动\-更新模式，必须用以下方法将 CAU 群集的角色添加到群集。 CAU 自我\-更新功能执行方式类似于任何其他群集工作负载，并且它可以与更新协调器计算机的计划内和计划外故障转移无缝协作。  
  
**远程\-更新**，可以随时从运行 Windows 或 Windows Server 的计算机启动更新运行。 可以启动更新运行群集感知更新窗口也可以使用**Invoke\-CauRun** PowerShell cmdlet。 远程\-更新是适用于 CAU 更新模式的默认值。 你可使用任务计划程序从不属于群集节点的远程计算机对所需的计划运行 [Invoke-CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-caurun) cmdlet。  
  
## <a name="can-i-schedule-updates-to-apply-during-a-backup"></a>可以计划要在备份过程应用更新？  
是。 CAU 在这方面不会施加任何约束。 但是，在服务器上执行软件更新\(相关联的可能重启\)服务器备份过程中进度不是 IT 的最佳做法。 请注意：CAU 只会依赖群集 API 确定资源故障转移和故障回复；因此，CAU 不知道服务器备份状态。  
  
## <a name="can-cau-work-with-system-center-configuration-manager"></a>CAU 是否可使用 System Center Configuration Manager？  
CAU 是一款协调群集节点上的软件更新和配置管理器也能执行服务器软件更新。 若要配置这些工具，使其不具有重叠的任何数据中心部署，包括使用不同的 Windows Server Update Services 服务器中的相同服务器至关重要。 这可确保使用 CAU 的目标不会无意破坏，因为 Configuration Manager\-驱动更新不会融入群集意识。  
  
## <a name="do-i-need-administrative-credentials-to-run-cau"></a>我是否需要管理凭据才能运行 CAU？  
是。 为了运行 CAU 工具，CAU 需要本地服务器的管理凭据或需要运行所在本地服务器或客户端计算机的“身份验证后模拟客户端”用户权限。 但是，若要协调群集节点上的软件更新，CAU 需要每个节点上的群集管理凭据。 尽管 CAU UI 无需凭据可以启动，但它会提示输入群集管理凭据连接到群集实例以预览或应用更新时。  
  
## <a name="can-i-script-cau"></a>可以编写脚本 CAU？  
是。 CAU 附带提供了一套丰富的脚本编写选项的 PowerShell cmdlet。 其中有和 CAU UI 为了执行 CAU 操作所调用的相同的 cmdlet。  

## <a name="what-happens-to-active-clustered-roles"></a>活动群集角色会发生什么情况？

群集角色\(以前称为应用程序和服务\)活动节点上，软件更新开始之前故障转移到其他节点。 CAU 通过使用维护模式来编排这些故障转移，该模式可暂停和耗尽节点的所有活动群集角色。 当软件更新完成时，CAU 恢复节点，群集角色会故障回复到更新节点。 这就确保相对于节点的群集角色分布在一个群集的 CAU 更新运行中保持相同。  
  
## <a name="how-does-cau-select-target-nodes-for-clustered-roles"></a>CAU 如何选择目标节点的群集角色？

CAU 依赖群集 API 协调故障转移。 聚类分析的 API 实现依赖于内部指标和智能位置启发来选择目标节点\(等工作负荷级别\)跨目标节点。  
  
## <a name="does-cau-load-balance-the-clustered-roles"></a>CAU 负载是否平衡的群集的角色？

CAU 不会加载的平衡群集的节点，但它会尝试保留群集角色分布。 当 CAU 完成群集节点的更新时，它会尝试将以前托管的群集角色的故障回复到该节点。 CAU 依赖群集 API 将资源故障回复到暂停程序的开始处。 由于没有计划外故障转移和首选所有者设置，群集角色的分布会保持不变。  
  
## <a name="how-does-cau-select-the-order-of-nodes-to-update"></a>CAU 如何选择节点的更新顺序？  
默认情况下，CAU 会根据活动的级别选择节点的更新顺序。 托管最少群集角色节点会最先更新。 但是，管理员可以指定特定的顺序的节点更新为更新运行 CAU UI 中指定的参数或使用 PowerShell cmdlet。  
  
## <a name="what-happens-if-a-cluster-node-is-offline"></a>如果群集节点处于脱机状态，会发生什么情况？

启动更新运行的管理员可指定能脱机的节点数量的可接受阈值。 因此，即使所有群集节点都没有联机，更新运行仍然可以在群集上继续。  
  
## <a name="can-i-use-cau-to-update-only-a-single-node"></a>可以使用 CAU 更新单个节点？  
否。 CAU 是群集\-作用域内更新工具，因此它只允许您选择要更新的群集。 如果希望更新单独一个节点，你可使用 CAU 以外的已有服务器工具。  
  
## <a name="can-cau-report-updates-that-are-initiated-from-outside-cau"></a>CAU 可以报告从 CAU 之外启动的更新？  
否。 CAU 仅可报告在 CAU 内启动的更新运行。 但是，后续 CAU 更新运行启动时，通过非已安装的更新\-CAU 方法相应地被视为以确定可能适用于每个群集节点的其他更新。  
  
## <a name="can-cau-support-my-unique-it-process-needs"></a>可以 CAU 支持我的独特 IT 程序需求？  
是。 CAU 提供以下尺度的灵活性来满足企业客户的独特 IT 程序需求：  
  
**脚本**更新运行可以指定预\-更新 PowerShell 脚本和 post\-更新 PowerShell 脚本。 预\-更新脚本会运行每个群集节点上之前暂停节点。 开机自检\-更新脚本的节点更新安装后每个群集节点上运行。  
  
> [!NOTE]  
> 必须在你想要运行预每个群集节点上安装.NET framework 4.6 或 4.5 和 PowerShell\-更新并发布\-更新脚本。 您还必须启用在群集节点上的 PowerShell 远程处理。 有关详细的系统要求，请参阅[要求和群集感知更新的最佳做法](cluster-aware-updating-requirements.md)。  
  
**高级更新运行选项**此外，管理员可以指定从大量高级更新运行的选项，例如每个节点重试更新过程的最大次数。 可以使用 CAU UI 或 CAU PowerShell cmdlet 指定这些选项。 这些自定义设置可以保存在更新运行配置文件中，供以后执行更新运行时重复使用。  
  
**公共插件\-体系结构中**CAU 包含注册、 注销的功能和选择插入\-项。CAU 附带两个默认插件\-项： 一个协调 Windows 更新代理\(WUA\) Api 上每个群集节点; 第二个应用手动复制到可供群集节点访问的文件共享的修补程序。 如果企业具有独特的需求不能满足这些的两个插入\-项，企业可以构建新的 CAU 插件\-在根据公共 API 规范。 有关详细信息，请参阅[群集\-感知更新插件\-参考中](https://msdn.microsoft.com/library/hh418084(VS.85).aspx)。  
  
有关配置和自定义 CAU 信息插入\-项以支持不同更新方案，请参阅[如何插入\-项工作](assetId:///847b571b-12b3-473c-953f-75a5a1f51333)。  
  
## <a name="how-can-i-export-the-cau-preview-and-update-results"></a>如何导出 CAU 预览和更新结果？  
CAU 提供导出选项通过命令\-行接口和通过用户界面。  
  
**命令\-行接口选项：**  
  
-   预览 results by using PowerShell cmdlet **Invoke\-CauScan |ConvertTo\-Xml**。 输出:XML  
  
-   通过使用 PowerShell cmdlet 来报告结果**Invoke\-CauRun |ConvertTo\-Xml**。 输出:XML  
  
-   通过使用 PowerShell cmdlet 来报告结果**获取\-CauReport |导出\-CauReport**。 输出:HTML、CSV  
  
**UI 选项：**  
  
-   从 **“预览更新”** 屏幕复制报告结果。 输出:CSV  
  
-   从 **“生成报告”** 屏幕复制报告结果。 输出:CSV  
  
-   从 **“生成报告”** 屏幕导出报告结果。 输出:HTML  
  
## <a name="how-do-i-install-cau"></a>如何安装 CAU？  
CAU 安装已无缝集成到故障转移群集功能中。 CAU 安装方式如下：  
  
-   故障转移群集上安装时的群集节点，CAU Windows Management Instrumentation \(WMI\)自动安装提供程序。  
  
-   在服务器或客户端计算机上安装故障转移群集工具功能时，将自动安装的群集感知更新 UI 和 PowerShell cmdlet。
  
## <a name="does-cau-need-components-running-on-the-cluster-nodes-that-are-being-updated"></a>CAU 是否需要在正在更新群集节点上运行的组件？  
CAU 不需要在群集节点上运行的服务。 但是，CAU 需要的软件组件\(WMI 提供程序\)群集节点上安装。 此组件是使用故障转移群集功能安装的。  
  
若要启用自动\-更新模式，CAU 群集的角色必须也添加到群集。  
  
## <a name="what-is-the-difference-between-using-cau-and-vmm"></a>使用 CAU 和 VMM 之间的区别是什么？  
  
-   System Center Virtual Machine Manager \(VMM\)的重点是更新的唯一超\-V 群集，而 CAU 可以更新任何一种受支持的故障转移群集，包括超\-V 群集。  
  
-   VMM 需要额外的许可，而 CAU 已获得所有 Windows Server 都许可。 CAU 功能、工具和 UI 是使用故障转移群集组件安装的。  
  
-   如果你已经拥有 System Center 许可证，你可以继续使用 VMM 更新超\-V 群集，因为它提供了集成的管理和软件更新体验。  
  
-   仅在运行 Windows Server 2016、 Windows Server 2012 R2 和 Windows Server 2012 的群集上支持 CAU。 VMM 还支持超\-V 群集运行 Windows Server 2008 R2 和 Windows Server 2008 的计算机上。  
  
## <a name="can-i-use-remote-updating-on-a-cluster-that-is-configured-for-self-updating"></a>可以使用远程\-更新为自动配置的群集上\-更新？  
是。 在自中的故障转移群集\-更新配置可以更新通过远程\-上的更新\-要求，就像您可以在计算机上强制 Windows 更新扫描在任何时间，即使 Windows 更新配置为自动安装更新。 但是，你需要确保更新运行没有已在进行中。  
  
## <a name="can-i-reuse-my-cluster-update-settings-across-clusters"></a>我能不能在群集间重复使用群集更新设置。  
是。 CAU 支持很多可以确定更新运行在其更新群集时的行为情况的更新运行选项。 这些选项可被另存为“更新运行配置文件”，并且可以在任何群集间重复使用。 我们建议你保存并在拥有相似更新需求的故障转移群集之间重复使用你的设置。 例如，可以创建"业务\-关键 SQL Server 群集正在更新运行配置文件"支持业务的所有 Microsoft SQL Server 群集的\-关键服务。  
  
## <a name="where-is-the-cau-plug-in-specification"></a>其中，是 CAU 插件\-规范中？  
  
-   [群集\-感知更新插件\-中引用](https://msdn.microsoft.com/library/hh418084(VS.85).aspx)  
  
-   [群集感知更新插件\-中示例](https://code.msdn.microsoft.com/windowsdesktop/Cluster-Aware-Updating-6a8854c9)  
  
## <a name="see-also"></a>请参阅  
  
-   [群集\-感知更新概述](cluster-aware-updating.md)  
  
