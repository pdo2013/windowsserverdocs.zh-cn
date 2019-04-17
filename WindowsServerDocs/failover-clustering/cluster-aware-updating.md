---
title: "群集的更新概述"
ms.topic: article
ms.prod: windows-server-threshold
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 3/23/2017
description: "群集的更新 (CAU) 可以自动上运行 Windows Server 群集的软件更新安装。"
ms.assetid: 3c2993b4-aa81-452b-a5c3-3724ad95d892
ms.openlocfilehash: 68336741accabd6e4cb0da5dbd708be707f68df6
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="cluster-aware-updating-overview"></a>群集的更新概述

> 适用于：Windows Server（半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主题提供概述 Cluster\ 感知更新 \(CAU\) 可以自动更新聚集服务器上的过程，同时保持可用性的软件功能。

> [!NOTE]
> 在更新时[存储空间直通 ](../storage/storage-spaces/storage-spaces-direct-overview.md)群集，我们建议使用群集的更新。
  
## <a name="BKMK_OVER"></a>功能描述  
群集的更新为自动功能，可使你更新中的服务器[故障转移群集](failover-clustering-overview.md)弱或者没有可用性在更新过程中会丢失。 在运行更新期间群集的更新坦诚执行下面任务：  

1. 将置于节点维护模式的每个群集节点。
2. 移动群集节点关闭角色。
3. 安装更新以及相关的任何更新。
4. 如有必要，请执行重新启动。
5. 带来利用维护模式节点。
6. 将恢复节点上的群集的角色。
7. 移动以进行更新下一步节点。

对于许多群集中的群集角色，自动更新过程触发计划的故障转移。 这可能导致连接客户的瞬时服务中断。 但是，在不断提供的工作负载，如 Hyper\-使用实时迁移 V 或与 SMB 透明故障转移、文件服务器的情况下群集的更新可以协调群集更新，而不会向服务可用性影响。    
  
## <a name="practical-applications"></a>实际应用  
  
-   CAU 减少了群集服务中的服务中断，减少了手动更新解决方法，并使 to\ end\ 端群集管理员更新更可靠的过程。 CAU 功能持续可用的群集工作负载，如持续可用的文件服务器的结合使用时 \（文件服务器工作负载 SMB 透明 Failover\ 与）或 Hyper\ V，群集服务可用性对零造成影响的客户端，可以执行更新。  
  
-   CAU 整个企业便于一致 IT 进程采用。 正在运行的配置文件更新可以为不同类别的故障转移群集创建和集中上的文件共享，以确保 CAU 部署整个 IT 部门统一，应用更新，即使群集由其他 lines\ of\-业务或系统管理员管理。  
  
-   CAU 可以计划更新上运行的每日、周，或每月定期以帮助协调群集与其他 IT 管理流程的更新。  
  
-   CAU 提供可扩展的体系结构更新群集软件库存 cluster\ 注意到的方式。 这可用于受发布者坐标安装的软件更新，不会发布到 Windows 更新或 Microsoft 更新或不提供来自 Microsoft，例如，non\ Microsoft 设备驱动程序的更新。  
  
-   CAU self\ 更新模式允许"框中的群集"装置 \（群集物理机，通常打包在一个 chassis\ 一组）更新本身。 通常情况下，此类装置部署在本地 IT 支持管理群集分支机构。 Self\ 更新模式提供了很大帮助在这些方案中部署。  
  
## <a name="important-functionality"></a>重要的功能  
以下是功能的重要更新群集的说明：  
  
-   用户界面 \(UI\)-群集注意到更新窗口-和 cmdlet，你可使用预览，应用，请监视器，并报告更新一组  
  
-   Cluster\ 更新操作 to\ end\ 端自动化 \ (更新 Run\)，统一协调由一个或多个更新处理协调器计算机  
  
-   集成 Microsoft 的重要更新的应用的现有的 Windows 更新代理 \(WUA\) 和 Windows Server Update Services \(WSUS\) 基础 Windows Server plug\ 在默认设置  
  
-   第二个 plug\ 中，可用于应用 Microsoft 的修复程序和，可以自定义应用 non\ Microsoft 更新  
  
-   更新运行配置文件，您将配置更新运行的选项，如将每个节点重试更新的最大次数的设置。 更新运行配置文件，可以快速跨运行更新重复使用相同的设置并轻松地与其他故障转移群集共享更新设置。  
  
-   支持新 plug\ 在开发，跨群集，如自定义软件安装程序，更新工具，BIOS 坐标其他 node\ 更新工具和网络适配器或主机总线适配器 \(HBA\) 更新工具可扩展体系结构。  
  
群集的更新可以协调完整的群集更新以下两种模式的操作：  
  
-   **Self\ 更新模式**此模式中，CAU 聚集的角色配置用作进行更新，而是故障转移群集上的工作负荷并且定义关联的更新计划。 群集更新本身在计划通过使用默认或自定义更新运行配置文件的时间。 运行更新期间 CAU 更新处理协调器开始在当前拥有 CAU 聚集的角色，节点，然后进程按顺序在每个群集节点执行更新。 若要更新的当前的群集节点，CAU 聚集的角色故障转移到另一个群集节点，并该节点一个新的更新处理协调器进程假定更新运行的控制。 在 self\ 更新模式下 CAU 可以故障转移群集使用更新完全自动化、to\ end\ 端更新的过程。 此外可以管理员触发器更新 on\ 也有按需在此模式中，或只需使用 remote\ 更新接近如果所需。 在 self\ 更新模式下管理员才能更新运行时的摘要信息进度通过连接到群集并运行**Get\ CauRun** Windows PowerShell cmdlet。  
  
-   **Remote\ 更新模式**此模式中，通过 CAU 工具配置远程计算机，称为更新协调。 更新处理协调器不更新运行期间更新群集的成员。 远程计算机上，从管理员触发 on\ 点播更新运行通过使用默认或自定义更新运行配置文件。 Remote\ 更新模式是用于监控运行更新期间 real\ 时间进度和群集服务器 Core 安装上运行。  
  
## <a name="hardware-and-software-requirements"></a>硬件和软件要求  

CAU 可在 Windows Server，包括服务器核心安装的所有版本。 要求的详细的信息，请参阅[群集的更新要求和最佳做法](cluster-aware-updating-requirements.md)。

### <a name="installing-cluster-aware-updating"></a>安装群集的更新
若要使用 CAU，安装在 Windows Server 故障转移群集的功能，并创建故障转移群集。 支持 CAU 功能组件上每个群集节点自动安装。

若要安装故障转移群集的功能，你可以使用以下工具：
- 在服务器管理器中添加功能向导中的角色和
- [安装 WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/server-manager/install-windowsfeature) Windows PowerShell cmdlet
- 部署映像服务和管理(DISM) 的命令行工具

有关详细信息，请参阅[安装或卸载角色角色服务或功能](https://technet.microsoft.com/library/hh831809(v=ws.11).aspx)。

您还必须安装故障转移群集工具，它是远程服务器管理工具的一部分，当你安装故障转移群集功能服务器管理器中默认情况下安装。 这些故障转移群集工具包括群集的更新用户界面和 PowerShell cmdlet。 

你必须安装故障转移群集工具，如下所示，以支持不同 CAU 更新模式：

- 若要使用 self\ 更新型 CAU，请在每个群集节点上安装故障转移群集工具。   
  
- 若要启用 remote\ 更新模式，请在具有网络连接到故障转移群集的计算机上安装故障转移群集工具。  
  
> [!NOTE]  
> -   无法使用 Windows Server 2012 上故障转移群集工具若要管理在较新版本的 Windows Server 的群集的更新。 
> -   若要仅在 remote\ 更新模式下使用 CAU 的故障转移群集工具群集节点不需要安装。 但是，某些 CAU 功能将不可用。 有关详细信息，请参阅[要求和最佳做法 Cluster\ 感知更新](cluster-aware-updating-requirements.md)。  
> -   除非你使用的 CAU 仅在 self\ 更新模式下，的计算机在其安装了这些 CAU 工具和用于协调更新不能故障转移群集的成员。  
  
### <a name="enabling-self-updating-mode"></a>启用自行更新模式
若要启用自行更新模式，你必须向故障转移群集添加群集的更新聚集的角色。 若要执行此操作，请使用以下方法之一：
- 在服务器管理器中，选择**工具** > **群集的更新**，然后在群集的更新窗口中，选择**自行更新选项的配置群集**。 
- 在 PowerShell 会话中，运行[添加 CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/add-cauclusterrole) cmdlet。  
  
若要卸载 CAU、使用服务器管理器中，卸载故障转移群集功能或故障转移群集工具[卸载 WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/server-manager/uninstall-windowsfeature) cmdlet，或 DISM command\ 行工具。  
  
### <a name="additional-requirements-and-best-practices"></a>其他要求和最佳做法  

若要确保成功，以及其他指南配置为使用 CAU 你故障转移群集环境，CAU 可以更新群集节点，可以运行 CAU 的最佳实践分析。  
  
详细的要求和最佳做法，使用 CAU，以及有关运行 CAU 的最佳实践分析的信息，请参阅[要求和最佳做法 Cluster\ 感知更新](cluster-aware-updating-requirements.md)。  
  
### <a name="starting-cluster-aware-updating"></a>启动群集的更新  
  
##### <a name="to-start-cluster-aware-updating-from-server-manager"></a>若要开始群集的更新中服务器管理器  
  
1.  开始服务器管理器。  
  
2.  请执行以下任一操作：  
  
    -   在**工具**菜单上，单击**Cluster\ 感知更新**。  
  
    -   如果一个或多个群集节点或群集，添加了服务器管理器中，在**所有服务器**页面，right\ 单击一个节点名称 \（或 cluster\ 的名称），然后单击**更新群集**。  
  
## <a name="see-also"></a>请参阅  
下面的链接将提供有关使用群集的更新的详细信息。  
  
-   [要求和 Cluster\ 意识到更新的最佳做法](cluster-aware-updating.md)  
  
-   [感知 Cluster\ 更新：常见问题](cluster-aware-updating-faq.md)  
  
-   [高级的选项和更新为 CAU 的运行配置文件](cluster-aware-updating-options.md)  
  
-   [CAU Plug\ 接的工作原理](cluster-aware-updating-plug-ins.md)  
  
-   [在 Windows PowerShell Cluster\ 感知更新 Cmdlet](https://go.microsoft.com/fwlink/p/?LinkId=237675)  
  
-   [Cluster\ 注意的更新中 Plug\ 参考](https://msdn.microsoft.com/library/hh418084.aspx)  
  

