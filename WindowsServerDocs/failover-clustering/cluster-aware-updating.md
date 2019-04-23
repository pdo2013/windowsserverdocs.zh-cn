---
title: 群集感知更新概述
ms.topic: article
ms.prod: windows-server-threshold
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 08/06/2018
description: 群集感知更新 (CAU) 会自动将运行 Windows Server 的群集上安装软件更新。
ms.assetid: 3c2993b4-aa81-452b-a5c3-3724ad95d892
ms.openlocfilehash: 77ccfe7dc9a769d602ff069d5f1d8e77522aa001
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827418"
---
# <a name="cluster-aware-updating-overview"></a>群集感知更新概述

> 适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

本主题概述群集\-感知更新\(CAU\)，同时保持可用性的一项功能，可自动执行软件更新过程，在群集服务器。

> [!NOTE]
> 更新时[存储空间直通](../storage/storage-spaces/storage-spaces-direct-overview.md)群集，我们建议使用群集感知更新。
  
## <a name="BKMK_OVER"></a>功能说明  
群集感知更新是一个自动化的功能，可用于更新中的服务器[故障转移群集](failover-clustering-overview.md)在更新过程期间的可用性损失极小或没有使用。 更新运行期间，群集感知更新以透明方式执行以下任务：  

1. 每个群集节点置于节点维护模式。
2. 将移出节点的群集的角色。
3. 安装更新和所有从属更新。
4. 如有必要，请执行重新启动。
5. 使节点退出维护模式。
6. 还原的节点上的群集的角色。
7. 继续更新下一个节点。

对于群集中的许多群集角色，自动更新过程将触发计划的故障转移。 对连接的客户端而言，这可能导致暂时性服务中断。 但是，对于连续可用的工作负荷，例如超\-V 与实时迁移或文件服务器具有 SMB 透明故障转移，群集感知更新可以协调群集更新进行不影响服务可用性对。    
  
## <a name="practical-applications"></a>实际应用程序  
  
-   CAU 可减少群集服务中的服务中断，降低了手动更新解决方法，并使最终\-到\-端群集更新程序的可靠性为管理员。 当 CAU 功能与连续可用的群集工作负荷，如持续可用文件服务器一起使用时\(具有 SMB 透明故障转移的文件服务器工作负荷\)或超\-V，群集可以为客户端服务可用性零影响执行更新。  
  
-   CAU 方便在企业内部采用一致的 IT 流程。 更新运行配置文件可以创建为不同类的故障转移群集并随后用于确保 IT 组织内的 CAU 部署一致地应用更新，即使群集由不同的行的文件共享上集中管理\-的\-业务范围或管理员。  
  
-   CAU 可以计划按照每天、每周或每月的定期间隔运行更新运行，有助于协调群集更新和其他 IT 管理过程。  
  
-   CAU 提供可扩展的体系结构来更新群集中的群集软件清单\-感知的方式。 这可用于由发布者协调软件更新的未发布到 Windows Update 或 Microsoft Update 或未提供的 Microsoft，例如，更新的安装非\-Microsoft 设备驱动程序。  
  
-   CAU 自我\-更新模式允许"机箱内的群集"设备\(一整套的群集物理机，通常封装在一个底盘\)自行更新。 这类设备一般部署在只提供最少本地 IT 支持的分支机构中，用于管理群集。 自助\-更新模式提供了巨大的价值，在这些部署方案中。  
  
## <a name="important-functionality"></a>重要功能  
以下是重要的群集感知更新功能的说明：  
  
-   用户界面\(UI\) -群集感知更新窗口中-和一组可用于预览、 应用、 监视和报告更新的 cmdlet  
  
-   结束\-到\-结束群集的自动化\-更新操作\(更新运行\)，这些都是由一个或多个更新协调器计算机  
  
-   默认插件\-中，与现有的 Windows 更新代理集成\(WUA\)和 Windows Server Update Services \(WSUS\)应用重要的 Microsoft Windows Server 中的基础结构更新  
  
-   第二个插件\-中，可用于应用 Microsoft 修补程序和的可自定义以应用非\-Microsoft 更新  
  
-   你使用“更新运行”选项的设置所配置的“更新运行配置文件”，例如每个节点更新可以重试的最大次数。 “更新运行配置文件”允许你迅速在各个更新运行之间重复使用相同设置并与其他故障转移群集共享更新设置。  
  
-   支持新插件的可扩展体系结构\-中开发来协调其他节点\-更新工具跨群集，如自定义软件安装程序、 BIOS 更新工具和网络适配器或主机总线适配器\(HBA\)更新工具。  
  
群集感知更新可以协调完整的群集更新操作在两种模式：  
  
-   **自助\-更新模式**对于此模式下，CAU 群集的角色被配置为要更新的故障转移群集上的工作负荷和定义一个相关联的更新计划。 群集会通过使用默认或自定义更新运行配置文件在计划的时间自行更新。 在更新运行中，CAU 更新协调器进程会在当前拥有 CAU 群集角色的节点上启动，此进程就会按顺序在每个群集节点上执行更新。 为了更新当前的群集节点，CAU 群集角色将故障转移到另一个群集节点，该节点上新的更新协调器进程将接管更新运行的控制权。 在自助\-更新模式，CAU 可以更新故障转移群集使用完全自动化，，最终\-到\-最终更新过程。 管理员还可以在触发更新\-在此模式下，或只需使用远程\-如果所需的更新方法。 在自助\-更新模式，管理员可以获得有关更新运行的摘要信息正在进行中通过连接到群集并运行**获取\-CauRun** Windows PowerShell cmdlet。  
  
-   **远程\-更新模式**对于此模式下，远程计算机，这称为更新协调器，使用 CAU 工具配置。 更新协调器并不是在更新运行期间更新的群集的成员。 从远程计算机上，管理员将触发上\-要求通过使用默认或自定义更新运行配置文件的更新运行。 远程\-更新模式可用于监视实时\-时间进度，在更新运行中，并在服务器核心安装上运行的群集。  
  
## <a name="hardware-and-software-requirements"></a>硬件和软件要求  

CAU 可在所有版本的 Windows Server，包括服务器核心安装上。 有关详细的要求信息，请参阅[群集感知更新要求和最佳做法](cluster-aware-updating-requirements.md)。

### <a name="installing-cluster-aware-updating"></a>安装群集感知更新
若要使用 CAU，请在 Windows 服务器上安装故障转移群集功能并创建故障转移群集。 会在每个群集节点上自动安装支持 CAU 功能的组件。

若要安装故障转移群集功能，你可使用以下工具：
- 服务器管理器中的“添加角色和功能向导”
- [Install-windowsfeature](https://docs.microsoft.com/powershell/module/servermanager/Install-WindowsFeature?view=winserver2012r2-ps&viewFallbackFrom=win10-ps) Windows PowerShell cmdlet
- 部署映像服务和管理 (DISM) 命令行工具

有关详细信息，请参阅[安装故障转移群集功能](create-failover-cluster.md#install-the-failover-clustering-feature)。

此外必须安装故障转移群集工具后，这是远程服务器管理工具的一部分，默认情况下安装在服务器管理器安装故障转移群集功能时。 故障转移群集工具包括群集感知更新用户界面和 PowerShell cmdlet。 

必须按如下所示安装故障转移群集工具以支持不同的 CAU 更新模式：

- 若要使用 CAU 中自助\-更新模式下，每个群集节点上安装故障转移群集工具。   
  
- 若要启用远程\-更新模式，与故障转移群集拥有网络连接的计算机上安装故障转移群集工具。  
  
> [!NOTE]  
> -   不能使用 Windows Server 2012 上的故障转移群集工具来管理群集感知更新较新版本的 Windows Server 上。 
> -   若要使用 CAU 仅在远程\-更新模式，在群集节点上的故障转移群集工具不需要安装。 但是，某些 CAU 功能将不可用。 有关详细信息，请参阅[要求和为群集的最佳做法\-感知更新](cluster-aware-updating-requirements.md)。  
> -   除非你要使用 CAU 只在自助\-更新模式，在其安装了 CAU 工具并且协调更新的计算机不会故障转移群集的成员。  
  
### <a name="enabling-self-updating-mode"></a>启用自我更新模式
若要启用自我更新模式，必须将群集感知更新的群集的角色添加到故障转移群集中。 为此，请使用以下方法之一：
- 在服务器管理器中，选择**工具** > **群集感知更新**，然后在群集感知更新窗口中，选择**配置群集自我更新选项**. 
- 在 PowerShell 会话中，运行[Add-cauclusterrole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Add-CauClusterRole?view=win10-ps) cmdlet。  
  
若要卸载 CAU，请使用服务器管理器卸载故障转移群集功能或故障转移群集工具[Uninstall-windowsfeature](https://docs.microsoft.com/powershell/module/servermanager/Uninstall-WindowsFeature?view=win10-ps) cmdlet 或 DISM 命令\-行工具。  
  
### <a name="additional-requirements-and-best-practices"></a>其他要求和最佳做法  

若要确保 CAU 成功更新群集节点，以及获取有关配置故障转移群集环境以使用 CAU 的其他指导，你可以运行 CAU 最佳做法分析器。  
  
有关详细的要求和使用 CAU，以及有关运行 CAU 最佳实践分析工具信息的最佳实践，请参阅[要求和为群集的最佳做法\-感知更新](cluster-aware-updating-requirements.md)。  
  
### <a name="starting-cluster-aware-updating"></a>启动群集感知更新  
  
##### <a name="to-start-cluster-aware-updating-from-server-manager"></a>若要启动群集感知更新从服务器管理器  
  
1.  启动“服务器管理器”。  
  
2.  执行下列操作之一：  
  
    -   上**工具**菜单上，单击**群集\-感知更新**。  
  
    -   如果一个或多个群集节点，或者该群集上添加到服务器管理器中，**的所有服务器**页上，右\-单击节点的名称\(或群集的名称\)，然后单击**更新群集**。  
  
## <a name="see-also"></a>请参阅  
以下链接提供有关使用群集感知更新的详细信息。  
  
-   [要求和为群集的最佳做法\-感知更新](cluster-aware-updating.md)  
  
-   [群集\-感知更新：方面的常见问题](cluster-aware-updating-faq.md)  
  
-   [高级的选项和 cau 更新运行配置文件](cluster-aware-updating-options.md)  
  
-   [CAU 如何插入\-项工作](cluster-aware-updating-plug-ins.md)  
  
-   [群集\-感知更新 Windows PowerShell 中的 Cmdlet](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps&viewFallbackFrom=winserverr2-ps)  
  
-   [群集\-感知更新插件\-中引用](https://docs.microsoft.com/previous-versions/windows/desktop/mscs/cluster-aware-update-plug-in-interfaces-and-classes)  
  

