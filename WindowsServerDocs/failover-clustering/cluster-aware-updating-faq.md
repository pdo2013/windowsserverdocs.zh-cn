---
ms.assetid: 6416d125-bcaf-433d-971a-2f0283bca2c2
title: "群集的更新的常见问题"
ms.topic: article
ms.prod: storage-failover-clustering
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 4/28/2017
description: "有关在 Windows Server 群集的更新的常见问题的解答。"
ms.openlocfilehash: 8417ea8b6b76e16c3f4db3bac5062d90a8da3ff2
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="cluster-aware-updating-frequently-asked-questions"></a>群集的更新：常见问题

> 适用于：Windows Server（半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

[群集的更新](cluster-aware-updating.md)\(CAU\) 是一项功能，坐标所有服务器以一种更比计划进行故障转移群集节点不会影响服务可用性故障转移群集上的软件更新。 某些应用程序连续可用性功能 \ (使用实时迁移，Hyper\ V) 或与 SMB 透明 Failover\ SMB 3.x 文件服务器，如 CAU 可以协调群集自动更新服务可用性不会影响。

## <a name="does-cau-support-updating-storage-spaces-direct-clusters"></a>CAU 是否支持存储空间直通 群集的更新？  
是的。 CAU 支持更新[存储空间直通 ](../storage/storage-spaces/storage-spaces-direct-overview.md)群集部署类型无论：hyper 汇聚或汇聚。 具体来说，CAU 业务流程可确保，暂停每个群集节点等待基础的群集的存储空间，可正常运行。

## <a name="does-cau-work-with-windows-server-2008-r2-or-windows-7"></a>是否与 Windows Server 2008 R2 或 Windows 7 工作 CAU？  
不。 CAU 协调群集更新仅从运行 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows 10、Windows 8.1 或 Windows 8 计算机的操作。 正在更新故障转移群集必须运行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012。
  
## <a name="is-cau-limited-to-specific-clustered-applications"></a>CAU 仅限于群集的特定应用程序？  
不。 CAU 是独立于聚集应用程序的类型。 CAU 是层叠放置在顶部群集 Api 和 PowerShell cmdlet 的外部 cluster\ 更新解决方案。 正因为如此，CAU 可以协调配置 Windows Server 故障转移群集任何聚集应用的更新。  
  
> [!NOTE]  
> 当前，以下群集的工作负载经过测试和认证的 CAU: SMB，Hyper\ V、DFS 复制、DFS 命名空间、iSCSI 和 NFS。  
  
## <a name="does-cau-support-updates-from-microsoft-update-and-windows-update"></a>CAU 是否支持从 Microsoft 更新和 Windows 更新的更新？  
是的。 默认情况下，CAU 配置与 plug\ 入群集节点上使用 Windows 更新代理 \(WUA\) 实用程序 Api。 可以配置 WUA 基础结构，以指向 Microsoft 更新和 Windows 更新或 Windows Server Update Services \(WSUS\) 作为更新的源。  
  
## <a name="does-cau-support-wsus-updates"></a>CAU 是否支持 WSUS 更新？  
是的。 默认情况下，CAU 配置与 plug\ 入群集节点上使用 Windows 更新代理 \(WUA\) 实用程序 Api。 可以配置 WUA 基础结构，以作为更新的源指向 Microsoft 更新和 Windows 更新或 Windows Server Update Services 本地 \(WSUS\) 服务器。  
  
## <a name="can-cau-apply-limited-distribution-release-updates"></a>可以将有限的发布版本更新 CAU 应用？  
是的。 有限的发布版本 \(LDR\) 更新，也称为修补程序，不发布通过 Microsoft 更新或 Windows 更新，以便它们无法下载通过 Windows 更新代理 \(WUA\) plug\-，默认情况下使用 CAU。  
  
但是，CAU 包含其次 plug\-你可以选择将应用修补程序的更新。 Plug\ 在此修补程序还可以自定义应用 non\ Microsoft 驱动程序、固件和 BIOS 更新。  
  
## <a name="can-i-use-cau-to-apply-cumulative-updates"></a>可以使用 CAU 累积更新将应用？  
是的。 如果累积更新常规 distribution 发布更新或 LDR 更新，CAU 可以将它们应用。  
  
## <a name="can-i-schedule-updates"></a>计划更新？  
是的。 CAU 支持这两种允许更新安排下列更新模式：  
  
**Self\ 更新**群集更新本身根据定义个人资料和定期，如维护期间每月的支持。 你还可以开始 Self\ 更新按需运行随时。 若要启用 self\ 更新模式，必须向群集添加 CAU 聚集的角色。 CAU self\ 更新功能执行任何其他聚集工作负载，例如，它可以使用更新处理协调器计算机的计划和非计划故障转移无缝地工作。  
  
**Remote\ 更新**使您可以随时从计算机运行的 Windows 或 Windows Server 开始更新运行。 你可以开始运行通过群集的更新窗口或使用更新**Invoke\ CauRun** PowerShell cmdlet。 Remote\ 更新是为 CAU 更新模式默认行为。 你可以使用任务计划程序运行[调用 CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-caurun) cmdlet 计划所需的不是一个群集节点远程计算机。  
  
## <a name="can-i-schedule-updates-to-apply-during-a-backup"></a>我可以计划在更新期间备份应用？  
是的。 CAU 不会在这方面有任何限制。 但是，执行服务器上的软件更新 \（使用的关联潜在 restarts\) 服务器备份处于时进度并不 IT 最佳做法。 请注意，CAU 依赖于仅群集 Api 来确定在资源故障转移以及故障回复;因此，CAU 没有意识到服务器备份状态。  
  
## <a name="can-cau-work-with-system-center-configuration-manager"></a>可以 CAU 处理 System Center Configuration Manager？  
CAU 坐标群集节点上的软件更新的工具以及 Configuration Manager 还执行服务器软件更新。 请务必配置这些工具，以便没有重叠相同的服务器，在任何 datacenter 部署，包括使用不同的 Windows Server Update Services 服务器的覆盖范围。 这可确保使用 CAU 背后的目标是不无意中失败，因为配置 Manager\ 驱动更新不合并群集感知。  
  
## <a name="do-i-need-administrative-credentials-to-run-cau"></a>我是否需要管理运行 CAU 的凭据？  
是的。 CAU 运行 CAU 工具，需要在本地服务器上，管理凭据或它需要**模拟后身份验证的客户端**直接在本地服务器或正在运行的客户端计算机上的用户。 但是，为了协调群集节点上的软件更新，CAU 需要群集管理凭据每个节点。 尽管 CAU UI 可以开始不凭据的情况下，它会提示输入群集管理凭据时连接到的群集实例预览或应用更新。  
  
## <a name="can-i-script-cau"></a>是否可以编写脚本 CAU？  
是的。 CAU 附带 PowerShell cmdlet 提供了丰富的脚本选项。 以下是 CAU UI 通话来执行操作 CAU 相同 cmdlet。  

## <a name="what-happens-to-active-clustered-roles"></a>活动聚集角色会发生什么情况？

在聚集角色 \（以前称为应用程序和 services\）节点上的活动，可以开始软件更新前故障转移到其他节点。 CAU 协调使用维护模式下，它会暂停和消耗的所有活动聚集角色节点这些故障转移。 软件更新时，CAU 恢复节点并返回到更新节点失败聚集的角色。 这将确保，相对于节点聚集角色分发保持不变跨群集 CAU 更新运行。  
  
## <a name="how-does-cau-select-target-nodes-for-clustered-roles"></a>CAU 是如何选择目标角色群集节点的？

CAU 依赖于群集协调故障转移的 Api。 群集 API 实现通过依赖内部指标和智能定位启发选择目标节点 \（如工作负载 levels\) 跨目标节点。  
  
## <a name="does-cau-load-balance-the-clustered-roles"></a>CAU 加载是否平衡聚集的角色？

CAU 未加载的群集的节点，但它尝试保留的群集角色 distribution 的余额。 完成 CAU 更新群集节点后，它将尝试失败后之前托管与该节点聚集的角色。 CAU 依赖于群集 Api 失败返回到暂停进程开头的资源。 因此在没有意外故障转移和首选的所有者设置聚集角色分发应保持不变。  
  
## <a name="how-does-cau-select-the-order-of-nodes-to-update"></a>CAU 是如何选择节点的顺序，若要更新的？  
默认情况下，CAU 选择节点的顺序更新根据活动级别。 节点承载少群集的角色是第一次更新。 但是，管理员可以通过为更新运行 CAU UI 中指定参数或使用 PowerShell cmdlet 更新节点特定的顺序。  
  
## <a name="what-happens-if-a-cluster-node-is-offline"></a>如果群集节点处于脱机状态，会发生什么情况？

启动运行更新的管理员可以指定可以将离线状态下的节点数接受阈值。 因此，更新运行才能继续群集上即使群集节点未处于联机状态。  
  
## <a name="can-i-use-cau-to-update-only-a-single-node"></a>可以使用 CAU 更新只有一个节点？  
不。 CAU 是 cluster\ 范围更新工具，以便只允许你选择群集更新。 如果你想要更新的单个节点，你可以使用现有 server 更新 CAU 独立的工具。  
  
## <a name="can-cau-report-updates-that-are-initiated-from-outside-cau"></a>可以 CAU 报告从外部 CAU 启动的更新？  
不。 CAU 可以仅报告更新中的运行从内 CAU 启动。 但是，当启动后续 CAU 更新运行时，通过 non\ CAU 方法已安装的更新均相应地视为以确定可能适用于每个群集节点的其他更新。  
  
## <a name="can-cau-support-my-unique-it-process-needs"></a>可以我唯一 IT 过程需要 CAU 支持？  
是的。 CAU 提供以下尺寸的灵活性根据企业客户的唯一 IT 过程需要：  
  
**脚本**运行更新可以指定 pre\ 更新 PowerShell 脚本和 post\ 更新 PowerShell 脚本。 之前节点将暂停，pre\ 更新脚本在每个群集节点上运行。 安装节点更新后，在每个群集节点上运行 post\ 更新脚本。  
  
> [!NOTE]  
> .NET framework 4.6 或 4.5 和 PowerShell 必须安装在你想要运行的 pre\ 更新和 post\ 更新脚本每个群集节点。 您还必须启用群集节点上的 PowerShell 远程处理。 有关详细的系统要求，请参阅[要求和最佳做法群集的更新](cluster-aware-updating-requirements.md)。  
  
**高级更新运行选项**管理员此外可以从一大的每个节点重试更新进程的时间的最大数等更新运行的高级选项。 可以使用 CAU UI 或 CAU PowerShell cmdlet 指定这些选项。 这些自定义设置可以保存在更新运行个人资料，并重新用于以后更新运行。  
  
**公开 plug\ 中体系结构**CAU 包括注册，注销，功能，并选择 plug\ 宏 CAU 附带两个默认 plug\ 接：一个坐标每个群集节点; 上的 Windows 更新代理 \(WUA\) Api 第二个应用手动群集节点向所访问的文件共享到复制的修补程序。 如果在企业中有以下两个 plug\ 接无法满足的独特需求，企业可生成在公用 API 规范根据的 plug\ 新 CAU。 有关详细信息，请参阅[Cluster\ 感知更新 Plug\ 中引用](http://msdn.microsoft.com/library/hh418084(VS.85).aspx)。  
  
有关信息配置和自定义 CAU plug\ 接支持不同更新方案，请参阅[Plug\ 接的工作原理](assetId:///847b571b-12b3-473c-953f-75a5a1f51333)。  
  
## <a name="how-can-i-export-the-cau-preview-and-update-results"></a>如何导出 CAU 预览和更新结果？  
CAU 提供导出选项 command\ 行界面通过和 UI。  
  
**Command\ 行界面选项：**  
  
-   通过使用 PowerShell cmdlet 预览结果**Invoke\ CauScan |ConvertTo\ Xml**。 输出：XML  
  
-   通过使用 PowerShell cmdlet 报告结果**Invoke\ CauRun |ConvertTo\ Xml**。 输出：XML  
  
-   通过使用 PowerShell cmdlet 报告结果**Get\ CauReport |Export\ CauReport**。 输出：HTML CSV  
  
**UI 选项：**  
  
-   将复制的报告结果**预览更新**屏幕。 输出：CSV  
  
-   将复制的报告结果**生成报告**屏幕。 输出：CSV  
  
-   将从报告结果导出**生成报告**屏幕。 输出：HTML  
  
## <a name="how-do-i-install-cau"></a>如何安装 CAU？  
CAU 安装无缝集成故障转移群集的功能。 CAU 安装，如下所示：  
  
-   在故障转移群集上的群集节点安装，CAU Windows 管理检测 \(WMI\) 提供商自动安装。  
  
-   服务器或客户端计算机上安装故障转移群集工具功能后，将自动安装更新的群集的 UI 和 PowerShell cmdlet。
  
## <a name="does-cau-need-components-running-on-the-cluster-nodes-that-are-being-updated"></a>CAU 是否需要群集节点正在更新上运行的组件？  
CAU 不需要的群集节点上运行的服务。 但是，CAU 需要的软件组件 \ 群集节点上安装 (WMI provider\)。 此组件是故障转移群集的功能与安装。  
  
若要启用 self\ 更新模式，必须还添加到群集 CAU 聚集的角色。  
  
## <a name="what-is-the-difference-between-using-cau-and-vmm"></a>使用 CAU 和 VMM 之间的区别是什么？  
  
-   System Center 虚拟机管理器 \(VMM\) 专注于更新仅 Hyper\ V 群集，而 CAU 可以更新受支持的故障转移群集，包括 Hyper\ V 群集的任何类型。  
  
-   VMM 需要附加许可，而 CAU 许可适用于所有 Windows Server。 与故障转移群集组件安装 CAU 功能、工具和 UI。  
  
-   如果你已经拥有 System Center 许可，你可以继续使用 VMM 更新 Hyper\ V 群集，因为它能够提供集成的管理和更新体验的软件。  
  
-   仅在运行 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 的群集支持 CAU。 VMM 还支持在运行 Windows Server 2008 R2 和 Windows Server 2008 计算机上的群集 Hyper\ V。  
  
## <a name="can-i-use-remote-updating-on-a-cluster-that-is-configured-for-self-updating"></a>可以使用 remote\ 更新配置群集上 self\ 更新？  
是的。 可以通过 remote\ 更新 on\ 的要求，更新故障转移群集 self\ 更新配置中的，就像你可以在你的计算机上强制随时扫描 Windows 更新，即使配置 Windows 更新自动安装更新。 但是，你需要确保，更新运行尚不在进度。  
  
## <a name="can-i-reuse-my-cluster-update-settings-across-clusters"></a>我在群集重用我群集更新设置  
是的。 CAU 支持大量确定更新运行时行为方式它更新群集的更新运行选项。 这些选项可另存为更新运行个人资料，并且他们可以在任何群集重复使用。 我们建议你保存，并跨具有类似的更新需求的故障转移群集重复使用你的设置。 例如，你可能创建"Business\ 关键 SQL Server 群集更新运行配置文件"的支持 business\ 关键服务的所有 Microsoft 的 SQL Server 群集。  
  
## <a name="where-is-the-cau-plug-in-specification"></a>CAU plug\ 中规范在哪里？  
  
-   [Cluster\ 注意的更新中 Plug\ 参考](http://msdn.microsoft.com/library/hh418084(VS.85).aspx)  
  
-   [示例 plug\ 中群集注意到的更新](http://code.msdn.microsoft.com/windowsdesktop/Cluster-Aware-Updating-6a8854c9)  
  
## <a name="see-also"></a>请参阅  
  
-   [感知 Cluster\ 更新概述](cluster-aware-updating.md)  
  
