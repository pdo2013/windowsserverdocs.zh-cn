---
ms.assetid: 6416d125-bcaf-433d-971a-2f0283bca2c2
title: 群集感知更新-常见问题
ms.topic: article
ms.prod: windows-server
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 04/28/2017
description: 有关 Windows Server 中的群集感知更新的常见问题的解答。
ms.openlocfilehash: a08366c7e64d9612d63e348d4cecdb4b2389737a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361348"
---
# <a name="cluster-aware-updating-frequently-asked-questions"></a>群集感知更新：常见问题

> 适用于：Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

[群集感知更新](cluster-aware-updating.md)\(CAU @ no__t 是一种功能，用于协调故障转移群集中所有服务器上的软件更新，这种方式不会影响服务可用性，这种方式不会影响群集节点的计划内故障转移。 对于一些具有连续可用性功能的应用程序 @no__t 使用实时迁移的超级 @ no__t-1V，或具有 SMB 透明故障转移 @ no__t-2 的 SMB 1.x 文件服务器，CAU 可协调自动群集更新而不影响服务有.

## <a name="does-cau-support-updating-storage-spaces-direct-clusters"></a>CAU 是否支持更新存储空间直通群集？  
是。 CAU 支持更新[存储空间直通](../storage/storage-spaces/storage-spaces-direct-overview.md)群集，而不考虑部署类型：超聚合或聚合。 具体而言，CAU 业务流程可确保暂停每个群集节点等待底层群集存储空间正常。

## <a name="does-cau-work-with-windowsserver-2008r2-or-windows7"></a>CAU 是否可以与 Windows Server 2008 R2 或 Windows 7 一起使用？  
否。 CAU 仅在运行 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows 10、Windows 8.1 或 Windows 8 的计算机中协调群集更新操作。 正在更新的故障转移群集必须运行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012。
  
## <a name="is-cau-limited-to-specific-clustered-applications"></a>CAU 是否限于特定的群集应用程序？  
否。 CAU 对于群集应用程序类型是不可知的。 CAU 是一个外部群集 @ no__t-0updating 解决方案，它在群集 Api 和 PowerShell cmdlet 之上分层。 同样，CAU 可以协调 Windows Server 故障转移群集中配置的任何群集应用程序的更新。  
  
> [!NOTE]  
> 目前，对以下群集工作负载进行 CAU 测试和认证：SMB、超级 @ no__t-0V、DFS 复制、DFS 命名空间、iSCSI 和 NFS。  
  
## <a name="does-cau-support-updates-from-microsoft-update-and-windows-update"></a>CAU 是否支持从 Microsoft Update 和 Windows Update 更新？  
是。 默认情况下，CAU 配置有一个插件 @ no__t-0in，该插件使用群集节点上的 Windows 更新代理 \(WUA @ no__t 实用工具 Api。 WUA 基础结构可配置为指向 Microsoft 更新和 Windows 更新或 Windows Server Update Services \(WSUS @ no__t 作为其更新源。  
  
## <a name="does-cau-support-wsus-updates"></a>CAU 是否支持 WSUS 更新？  
是。 默认情况下，CAU 配置有一个插件 @ no__t-0in，该插件使用群集节点上的 Windows 更新代理 \(WUA @ no__t 实用工具 Api。 WUA 基础结构可配置为指向 Microsoft 更新和 Windows 更新或本地 Windows Server Update Services \(WSUS @ no__t 服务器作为其更新源。  
  
## <a name="can-cau-apply-limited-distribution-release-updates"></a>CAU 是否可应用有限分发版本更新？  
是。 有限分发版本 \(LDR @ no__t 更新（也称为修补程序）不通过 Microsoft 更新或 Windows 更新发布，因此它们不能由 Windows 更新代理 \(WUA @ no__t-3 @ no__t-4in 默认情况下使用 CAU.  
  
但是，CAU 包含另一个即插 @ no__t-0in，你可以选择它来应用修补程序更新。 此修补程序插件还可以自定义为应用非 @ no__t 1Microsoft 驱动程序、固件和 BIOS 更新。 no__t  
  
## <a name="can-i-use-cau-to-apply-cumulative-updates"></a>我是否可以使用 CAU 来应用累积更新？  
是。 如果累积更新是常规的分发版本更新或 LDR 更新，CAU 可以应用它们。  
  
## <a name="can-i-schedule-updates"></a>能否计划更新？  
是。 CAU 支持以下更新模式，都允许对更新进行计划：  
  
**Self @ no__t-1updating**允许群集根据定义的配置文件和定期计划（例如在每月维护时段内）更新自身。 你还可以随时按需启动 "自行 @ no__t-0Updating"。 若要启用 @ no__t-0updating 模式，你必须将 CAU 群集角色添加到群集。 CAU self @ no__t-0updating 功能的执行方式与其他任何群集工作负荷一样，可与更新协调器计算机的计划内和计划外故障转移无缝协作。  
  
**Remote @ no__t-1updating**使你能够在运行 Windows 或 Windows Server 的计算机上随时启动更新运行。 可以通过群集感知更新窗口或使用**调用 @ no__t-1CauRun** PowerShell cmdlet 来启动更新运行。 Remote @ no__t-0updating 是 CAU 的默认更新模式。 你可使用任务计划程序从不属于群集节点的远程计算机对所需的计划运行 [Invoke-CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-caurun) cmdlet。  
  
## <a name="can-i-schedule-updates-to-apply-during-a-backup"></a>能否计划在备份期间应用更新？  
是。 在这方面，CAU 不会施加任何约束。 但是，在服务器上执行软件更新 \(with 在进行服务器备份时关联的可能重新启动 @ no__t-1 并不是最佳做法。 请注意：CAU 只会依赖群集 API 确定资源故障转移和故障回复；因此，CAU 不知道服务器备份状态。  
  
## <a name="can-cau-work-with-system-center-configuration-manager"></a>CAU 能否与 System Center Configuration Manager 配合工作？  
CAU 是一种工具，用于协调群集节点上的软件更新，并 Configuration Manager 还执行服务器软件更新。 必须配置这些工具，使其不会在任何数据中心部署中覆盖相同的服务器，包括使用不同 Windows Server Update Services 服务器。 这可以确保使用 CAU 后的目标不会不经意地失效，因为 Configuration Manager @ no__t-0driven 更新不会并入群集感知。  
  
## <a name="do-i-need-administrative-credentials-to-run-cau"></a>我是否需要管理凭据才能运行 CAU？  
是。 为了运行 CAU 工具，CAU 需要本地服务器的管理凭据或需要运行所在本地服务器或客户端计算机的“身份验证后模拟客户端”用户权限。 但是，若要协调群集节点上的软件更新，CAU 需要每个节点上的群集管理凭据。 尽管 CAU UI 无需凭据即可启动，但它会在连接到群集实例以预览或应用更新时提示输入群集管理凭据。  
  
## <a name="can-i-script-cau"></a>能否为 CAU 编写脚本？  
是。 CAU 附带 PowerShell cmdlet，提供一组丰富的脚本选项。 其中有和 CAU UI 为了执行 CAU 操作所调用的相同的 cmdlet。  

## <a name="what-happens-to-active-clustered-roles"></a>活动群集角色会发生什么情况？

群集角色 @no__t 名为 no__t 的应用程序和服务 @，在节点上处于活动状态，可以在软件更新开始之前故障转移到其他节点。 CAU 通过使用维护模式来编排这些故障转移，该模式可暂停和耗尽节点的所有活动群集角色。 当软件更新完成时，CAU 恢复节点，群集角色会故障回复到更新节点。 这就确保相对于节点的群集角色分布在一个群集的 CAU 更新运行中保持相同。  
  
## <a name="how-does-cau-select-target-nodes-for-clustered-roles"></a>CAU 如何选择群集角色的目标节点？

CAU 依赖群集 API 协调故障转移。 群集 API 实现通过依赖于内部度量值和智能放置试探法 \(such 作为工作负荷级别 @ no__t （跨目标节点）来选择目标节点。  
  
## <a name="does-cau-load-balance-the-clustered-roles"></a>CAU 负载是否会平衡群集角色？

CAU 不会对群集节点进行负载平衡，但它会尝试保留群集角色的分布。 当 CAU 完成群集节点的更新时，它会尝试将以前托管的群集角色的故障回复到该节点。 CAU 依赖群集 API 将资源故障回复到暂停程序的开始处。 由于没有计划外故障转移和首选所有者设置，群集角色的分布会保持不变。  
  
## <a name="how-does-cau-select-the-order-of-nodes-to-update"></a>CAU 如何选择节点的更新顺序？  
默认情况下，CAU 会根据活动的级别选择节点的更新顺序。 托管最少群集角色节点会最先更新。 但是，管理员可以通过在 CAU UI 中为更新运行指定参数或使用 PowerShell cmdlet 来指定更新节点的特定顺序。  
  
## <a name="what-happens-if-a-cluster-node-is-offline"></a>如果群集节点处于脱机状态，会发生什么情况？

启动更新运行的管理员可指定能脱机的节点数量的可接受阈值。 因此，即使所有群集节点都没有联机，更新运行仍然可以在群集上继续。  
  
## <a name="can-i-use-cau-to-update-only-a-single-node"></a>是否可以使用 CAU 仅更新单个节点？  
否。 CAU 是群集 @ no__t-0scoped 更新工具，因此它只允许你选择要更新的群集。 如果希望更新单独一个节点，你可使用 CAU 以外的已有服务器工具。  
  
## <a name="can-cau-report-updates-that-are-initiated-from-outside-cau"></a>是否可以从 CAU 外部启动 CAU 报表更新？  
否。 CAU 仅可报告在 CAU 内启动的更新运行。 但是，当后续的 CAU 更新运行启动时，将适当地考虑通过非 @ no__t-0CAU 方法安装的更新，以确定可能适用于每个群集节点的其他更新。  
  
## <a name="can-cau-support-my-unique-it-process-needs"></a>CAU 是否可以支持我的独特 IT 流程需求？  
是。 CAU 提供以下尺度的灵活性来满足企业客户的独特 IT 程序需求：  
  
**脚本**更新运行可以指定前 @ no__t-1update PowerShell 脚本和 post @ no__t-2update PowerShell 脚本。 在暂停节点之前，pre @ no__t-0update 脚本在每个群集节点上运行。 安装节点更新后，在每个群集节点上运行 post @ no__t-0update 脚本。  
  
> [!NOTE]  
> 必须在要运行 no__t-0update 和 post @ no__t-1update 脚本的每个群集节点上安装 .NET Framework 4.6 或4.5 和 PowerShell。 还必须在群集节点上启用 PowerShell 远程处理。 有关详细的系统要求，请参阅[群集感知更新的要求和最佳方案](cluster-aware-updating-requirements.md)。  
  
**高级更新运行选项**管理员还可以从大量高级更新运行选项中进行指定，例如每个节点上重试更新过程的最大次数。 可以使用 CAU UI 或 CAU PowerShell cmdlet 指定这些选项。 这些自定义设置可以保存在更新运行配置文件中，供以后执行更新运行时重复使用。  
  
**Public a @ no__t-1in 体系结构**CAU 包含注册、注销和选择插件 @ no__t-2ins 的功能。CAU 附带两个默认的即插即用 @ no__t-0ins：一个在每个群集节点上协调 Windows 更新代理 \(WUA @ no__t Api;第二个应用手动复制到群集节点可访问的文件共享的修补程序。 如果企业有这两个 no__t-0ins 不能满足的独特需求，企业可以根据公共 API 规范构建一个新的 CAU 插件 @ no__t-1in。 有关详细信息，请参阅[Cluster @ no__t-1Aware 更新插件 @ no__t-2In Reference](https://msdn.microsoft.com/library/hh418084(VS.85).aspx)。  
  
若要了解如何配置和自定义 CAU 插件 @ no__t-0ins 以支持不同的更新方案，请参阅[如何使用插件 @ no__t 2ins](assetId:///847b571b-12b3-473c-953f-75a5a1f51333)。  
  
## <a name="how-can-i-export-the-cau-preview-and-update-results"></a>如何导出 CAU 预览和更新结果？  
CAU 通过命令 @ no__t-0line 接口和 UI 提供导出选项。  
  
**Command @ no__t-1line interface options：**  
  
-   使用 PowerShell cmdlet**调用 @ no__t-1CauScan | 预览结果Convertto-html @ no__t-2Xml**。 输出:XML  
  
-   使用 PowerShell cmdlet**调用 @ no__t 报告结果-1CauRun |Convertto-html @ no__t-2Xml**。 输出:XML  
  
-   使用 PowerShell cmdlet **Get @ no__t-1CauReport 来报告结果 |Export @ no__t-2CauReport**。 输出:HTML、CSV  
  
**UI 选项：**  
  
-   从 **“预览更新”** 屏幕复制报告结果。 输出:CSV  
  
-   从 **“生成报告”** 屏幕复制报告结果。 输出:CSV  
  
-   从 **“生成报告”** 屏幕导出报告结果。 输出:HTML  
  
## <a name="how-do-i-install-cau"></a>如何安装 CAU？  
CAU 安装已无缝集成到故障转移群集功能中。 CAU 安装方式如下：  
  
-   在群集节点上安装故障转移群集时，将自动安装 CAU Windows Management Instrumentation \(WMI @ no__t 提供程序。  
  
-   当在服务器或客户端计算机上安装故障转移群集工具功能时，将自动安装群集感知更新 UI 和 PowerShell cmdlet。
  
## <a name="does-cau-need-components-running-on-the-cluster-nodes-that-are-being-updated"></a>CAU 是否需要正在更新的群集节点上运行的组件？  
CAU 不需要在群集节点上运行的服务。 但是，CAU 需要在群集节点上安装 0the WMI provider @ no__t 的软件 @no__t 组件。 此组件是使用故障转移群集功能安装的。  
  
若要启用 @ no__t-0updating 模式，还必须将 CAU 群集角色添加到群集。  
  
## <a name="what-is-the-difference-between-using-cau-and-vmm"></a>使用 CAU 和 VMM 之间的区别是什么？  
  
-   System Center Virtual Machine Manager @no__t 0VMM @ no__t 仅侧重于更新超级 @ no__t-Hqb-2v-fyv 群集，而 CAU 可以更新任何类型的受支持的故障转移群集，包括超级 @ no__t-3V 群集。  
  
-   VMM 需要额外的许可，而 CAU 已获得适用于所有 Windows Server。 CAU 功能、工具和 UI 是使用故障转移群集组件安装的。  
  
-   如果你已经拥有一个 System Center 许可证，则可以继续使用 VMM 更新超级 @ no__t-0V 群集，因为它提供集成的管理和软件更新体验。  
  
-   仅在运行 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 的群集上支持 CAU。 VMM 还支持运行 Windows Server 2008 R2 和 Windows Server 2008 的计算机上的超级 @ no__t-0V 群集。  
  
## <a name="can-i-use-remote-updating-on-a-cluster-that-is-configured-for-self-updating"></a>能否在配置为自行 @ no__t-1updating 的群集上使用远程 @ no__t-0updating？  
是。 可以通过 @ no__t-2demand 上的远程 @ no__t-1updating 更新 no__t 中的故障转移群集，就像你可以在计算机上随时强制进行 Windows 更新扫描，即使 Windows 更新配置为安装更新也是如此。都会. 但是，你需要确保更新运行没有已在进行中。  
  
## <a name="can-i-reuse-my-cluster-update-settings-across-clusters"></a>我能不能在群集间重复使用群集更新设置。  
是。 CAU 支持很多可以确定更新运行在其更新群集时的行为情况的更新运行选项。 这些选项可被另存为“更新运行配置文件”，并且可以在任何群集间重复使用。 我们建议你保存并在拥有相似更新需求的故障转移群集之间重复使用你的设置。 例如，你可能会为所有支持 Business @ no__t 1critical services 的 Microsoft SQL Server 群集创建 "Business @ no__t-0Critical SQL Server 群集更新运行配置文件"。  
  
## <a name="where-is-the-cau-plug-in-specification"></a>什么是 CAU 插件 @ no__t-0in 规范？  
  
-   [Cluster @ no__t-1Aware 更新插件 @ no__t-2in Reference](https://msdn.microsoft.com/library/hh418084(VS.85).aspx)  
  
-   [群集感知更新插件 @ no__t-1in 示例](https://code.msdn.microsoft.com/windowsdesktop/Cluster-Aware-Updating-6a8854c9)  
  
## <a name="see-also"></a>请参阅  
  
-   [Cluster @ no__t-1Aware 更新概述](cluster-aware-updating.md)  
  
