---
ms.assetid: b3d6fb87-c4d4-451c-b3de-a53d2402d295
title: 安装新的 Windows Server 2012 Active Directory 林（级别 200）
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 187db7e201e98ae97268b96c2e4faa202a9a5372
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874828"
---
# <a name="install-a-new-windows-server-2012-active-directory-forest-level-200"></a>安装新的 Windows Server 2012 Active Directory 林（级别 200）

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

本主题按入门级别说明了新的 Windows Server 2012 Active Directory 域服务域控制器升级功能。 在 Windows Server 2012 中，AD DS 使用服务器管理器和基于 Windows PowerShell 的部署系统替换 Dcpromo 工具。  
  
-   [Active Directory 域服务简化的管理](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SimplifiedAdmin)  
  
-   [技术概述](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_TechOverview)  
  
-   [部署林使用服务器管理器](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SMForest)  
  
-   [部署具有 Windows PowerShell 的林](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_PSForest)  
  
## <a name="BKMK_SimplifiedAdmin"></a>Active Directory 域服务简化的管理  
Windows Server 2012 引入了下一代 Active Directory 域服务简化管理，并且是 Windows 2000 Server 以来对域最彻底的重新构想。 AD DS 简化管理吸取 Active Directory 十二年的经验，为架构师和管理员创造更加可支持、更灵活、更直观的管理体验。 这意味着创建现有技术的新版本以及扩展在 Windows Server 2008 R2 中发布的组件的功能。  
  
### <a name="what-is-ad-ds-simplified-administration"></a>什么是 AD DS 简化管理？  
AD DS 简化管理是对域部署的重构。 这些功能包括：  
  
-   AD DS 角色部署现在是新服务器管理器体系结构的一部分，并且允许远程安装。  
  
-   AD DS 部署和配置引擎现在是 Windows PowerShell（即使在使用图形设置时）。  
  
-   升级现在包括先决条件检查，可验证林和域对新域控制器的准备情况，从而降低升级失败的概率。  
  
-   Windows Server 2012 林功能级别不实现新功能，只有新 Kerberos 功能的子集需要域功能级别，这使管理员不必总是需要同类的域控制器环境。  
  
### <a name="purpose-and-benefits"></a>目的和优势  
这些更改可能显得更复杂，而非更简单。 但在重新设计 AD DS 部署过程中，有机会将许多步骤和最佳实践合并为更少、更简单的操作。 例如，这意味着新副本域控制器的图形配置现在是八个对话框，而不是以前的十二个。 创建新的 Active Directory 林需要的 *单个* Windows PowerShell 命令，并且仅带有 *一个* 参数：域名。  
  
为何如此强调 Windows Server 2012 中的 Windows PowerShell？ 随着分布式计算的发展，Windows PowerShell 允许从图形和命令行界面进行配置和维护的单个引擎。 它允许通过 API 授予开发人员的相同 IT 专业人员第一类用户身份对任何组件进行完整功能的脚本编写。 随着基于云的计算变得无处不在，Windows PowerShell 最终还提供了远程管理服务器的功能，使没有图形界面的计算机具有与带显示器和鼠标的计算机相同的管理功能。  
  
资深 AD DS 管理员应该会发现他们以前所学的知识具有高度相关性。 初级管理员会发现学习曲线明显变浅。  
  
## <a name="BKMK_TechOverview"></a>技术概述  
  
### <a name="what-you-should-know-before-you-begin"></a>开始操作前你应该知道的内容  
本主题假定你熟悉 Active Directory 域服务之前的版本，并且不提供关于它们的目的和功能的基本详细信息。 有关 AD DS 的详细信息，请参阅 TechNet 门户页面，链接如下：  
  
-   [适用于 Windows Server 2008 R2 的 active Directory 域服务](https://technet.microsoft.com/library/dd378801(WS.10).aspx)  
  
-   [适用于 Windows Server 2008 的 active Directory 域服务](https://technet.microsoft.com/library/dd378891(WS.10).aspx)  
  
-   [Windows Server 技术参考](https://technet.microsoft.com/library/cc739127(WS.10).aspx)  
  
### <a name="functional-descriptions"></a>功能描述  
  
#### <a name="ad-ds-role-installation"></a>AD DS 角色安装  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_SelectServerRoles.gif)  
  
Active Directory 域服务安装使用服务器管理器和 Windows PowerShell，和 Windows Server 2012 中的所有其他服务器角色和功能一样。 Dcpromo.exe 程序不再提供 GUI 配置选项。  
  
在本地和远程安装中，你都可以使用服务器管理器或 Windows PowerShell 的 ServerManager 模块中的图形向导。 通过运行这些向导或 cmdlet 的多个实例并针对不同的服务器，你可以将 AD DS 同时部署到多个域控制器，全部操作都从单个控制台进行。 虽然这些新功能不与 Windows Server 2008 R2 或更早的操作系统向后兼容，你仍然可以将 Windows Server 2008 R2 中引入的 Dism.exe 应用程序从经典命令行进行本地角色安装。  
  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSAddWindowsFeature.png)  
  
#### <a name="ad-ds-role-configuration"></a>AD DS 角色配置  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_DeploymentConfiguration_Forest.gif)  
  
"以前称为 DCPROMO"的 active Directory 域服务配置为现在从角色安装的离散操作。 安装 AD DS 角色后，管理员使用服务器管理器内的单独向导或使用 ADDSDeployment Windows PowerShell 模块将服务器配置为域控制器。  
  
AD DS 角色配置在十二年行业经验的基础上构建，现在基于最新的 Microsoft 最佳实践配置域控制器。 例如，域名系统和全局编录默认在每个域控制器上安装。  
  
服务器管理器 AD DS 配置向导将许多单个对话框合并到更少的提示，并不再隐藏在"高级"模式下的设置。 整个升级过程是一个在安装期间不断扩大的对话框。 向导和 ADDSDeployment Windows PowerShell 模块向你显示重大更改和安全性注意事项，其中包括指向详细信息的链接。  
  
Dcpromo.exe 保留在 Windows Server 2012 中，仅用于命令行无人参与安装，并且不再运行图形安装向导。 强烈建议你不再将 Dcpromo.exe 用于无人参与的安装，并使用 ADDSDeployment 模块替代它，因为现在已弃用的可执行文件将不会包括在下一版本的 Windows 中。  
  
这些新功能不会对 Windows Server 2008 R2 或更早的操作系统进行向后兼容。  
  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDSForest.png)  
  
> [!IMPORTANT]  
> Dcpromo.exe 不再包括图形向导，并且不再安装角色或功能二进制文件。 尝试从资源管理器外壳程序运行 Dcpromo.exe 时将返回：  
>   
> "Active Directory 域服务安装向导是重新定位在服务器管理器。 有关详细信息，请参阅 https://go.microsoft.com/fwlink/?LinkId=220921。"  
>   
> 尝试运行 Dcpromo.exe /unattend 仍然可安装二进制文件，和之前的操作系统一样，但会发出警告：  
>   
> "Dcpromo 无人参与的操作适用于 Windows PowerShell 的 ADDSDeployment 模块替换为。 有关详细信息，请参阅 https://go.microsoft.com/fwlink/?LinkId=220924。"  
>   
> Windows Server 2012 弃用 dcpromo.exe，而且它将不包括在 Windows 的将来版本中，也不会在此操作系统中得到进一步增强。 管理员应停止使用它，并切换到支持的 Windows PowerShell 模块（如果他们希望从命令行创建域控制器）。  
  
#### <a name="prerequisite-checking"></a>先决条件检查  
域控制器配置还实现先决条件检查阶段，该阶段可在继续进行域控制器升级前评估林和域。 这包括 FSMO 角色可用性、用户权限、扩展的架构兼容性和其他要求。 这种新的设计可缓和域控制器升级开始后因为致命配置错误而中途暂停的问题。 这样可以减少域控制器元数据在林中被孤立或服务器错误地认为它是域控制器的几率。  
  
## <a name="BKMK_SMForest"></a>部署林使用服务器管理器  
本部分介绍如何在图形 Windows Server 2012 计算机上使用服务器管理器在目录林根级域中安装第一个域控制器。  
  
### <a name="server-manager-ad-ds-role-installation-process"></a>服务器管理器 AD DS 角色安装过程  
下图说明了 Active Directory 域服务角色安装过程，以你运行 ServerManager.exe 开始，在升级域控制器前结束。  
  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_servermanagerdeployment.png)  
  
#### <a name="server-pool-and-add-roles"></a>服务器池和添加角色  
可从运行服务器管理器的计算机访问的任何 Windows Server 2012 计算机都有资格添加到池。 在添加到池后，你可以选择这些服务器用于远程安装 AD DS 或任何在服务器管理器内可能出现的配置选项。  
  
若要添加服务器，请选择以下方法之一：  
  
-   单击仪表板欢迎磁贴上的“添加要管理的其他服务器”   
  
-   单击“管理”菜单并选择“添加服务器”  
  
-   右键单击“所有服务器”并选择“添加服务器”  
  
这将打开“添加服务器”对话框：  
  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddServers.png)  
  
这为你提供了将服务器添加到池以供使用或分组的三种方法：  
  
-   Active Directory 搜索（使用 LDAP，要求计算机属于某个域，允许操作系统筛选并支持通配符）  
  
-   DNS 搜索（通过 ARP 或 NetBIOS 广播或者 WINS 查找使用 DNS 别名或 IP 地址，不允许操作系统筛选或支持通配符）  
  
-   导入（使用以 CR/LF 分隔的服务器的文本文件列表）  
  
单击“立即查找”以从计算机加入的相同 Active Directory 域返回服务器列表，单击服务器列表中的一个或多个服务器名称。 单击右箭头以将服务器添加到“已选择”列表。 使用“添加服务器”对话框将选定的服务器添加到仪表板角色组。 或者单击“管理”，然后单击“创建服务器组”，或单击仪表板“欢迎使用服务器管理器”磁贴上的“创建服务器组”以创建自定义服务器组。  
  
> [!NOTE]  
> 添加服务器过程不验证服务器是否联机或可访问。 但是，任何无法访问的服务器都将在下一次刷新时在服务器管理器的“可管理性”视图中标记出来。  
  
你可以在添加到池中的任何 Windows Server 2012 计算机上远程安装角色，如下所示：  
  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/tADDS_SMI_TR_AddRolesFeatures.png)  
  
无法完全管理运行早于 Windows Server 2012 的操作系统的服务器。 “添加角色和功能”  选择运行 ServerManager Windows PowerShell 模块 **Install-WindowsFeature**。  
  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddADDSToAnotherServer.png)  
  
还可以使用现有域控制器上的服务器管理器仪表板来选择带有预选择角色的远程服务器 AD DS 安装，方法是右键单击 AD DS 仪表板磁贴并选择“将 AD DS 添加到另一个服务器” 。 这会调用 **Install-WindowsFeature AD-Domain-Services**。  
  
你正在运行服务器管理器的计算机自动将自身添加到池中。 若要在此处安装 AD DS 角色，只需单击“管理”  菜单并单击“添加角色和功能” 。  
  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ManageAddRoles.png)  
  
#### <a name="installation-type"></a>安装类型  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectInstallationType.png)  
  
“安装类型”  对话框提供一个选项，此选项不支持 Active Directory 域服务：“基于远程桌面服务方案的安装” 。 该选项仅允许多服务器分布式工作负载中的远程桌面服务。 如果你选择它，AD DS 将无法安装。  
  
安装 AD DS 时，始终不要更改默认选择：“基于角色或基于功能的安装”。  
  
#### <a name="server-selection"></a>服务器选择  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectDestinationServer.png)  
  
“服务器选择”对话框使你可以从之前添加到池的服务器中选择一个（只要它可访问）。 运行服务器管理器的本地服务器自动可用。  
  
此外，你可以选择使用 Windows Server 2012 操作系统的脱机 HYPER-V VHD 文件，服务器管理器直接通过组件服务将角色添加到它们中。 这使你可以在进一步配置它们之前使用必要的组件配置虚拟服务器。  
  
#### <a name="server-roles-and-features"></a>服务器角色和功能  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectServerRoles.png)  
  
如果你想要升级域控制器，请选择“Active Directory 域服务”。 所有 Active Directory 管理功能和所需服务都将自动安装，即使它们表面上是另一个角色的一部分，或者不在服务器管理器界面中显示为选中状态。  
  
服务器管理器还提供一个信息对话框，它显示此角色隐式安装的管理功能；这与 **-IncludeManagementTools** 参数等效。  
  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddFeaturesDialog.gif)  
  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectFeatures.png)  
  
此处可按需添加其他“功能”  。  
  
#### <a name="active-directory-domain-services"></a>Active Directory 域服务  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ADDSIntro.png)  
  
“Active Directory 域服务”对话框提供有关要求和最佳实践的有限信息。 它主要用于确认你已选择 AD DS 角色"如果未显示此屏幕，你未选择 AD DS。  
  
#### <a name="confirmation"></a>确认  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Confirmation.png)  
  
“确认”  对话框是角色安装开始前的最后一个检查点。 它提供在角色安装后根据需要重新启动计算机的选项，但是 AD DS 安装不需要重新启动。  
  
通过单击“安装”，即确认你已准备好开始角色安装。 一旦开始就无法取消角色安装。  
  
#### <a name="results"></a>结果  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Results.png)  
  
“结果”  对话框显示当前安装进度和当前安装状态。 无论服务器管理器是否关闭，角色安装都继续。  
  
验证安装结果仍然最佳实践。 如果在安装完成前关闭“结果”  对话框，你可以使用服务器管理器通知标志检查结果。 服务器管理器还为任何已安装 AD DS 但未进一步配置为域控制器的服务器显示警告消息。  
  
**任务通知**  
  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_TaskNotofications.png)  
  
**AD DS 的详细信息**  
  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ADDSDetails.png)  
  
**任务详细信息**  
  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_TaskDetails.png)  
  
#### <a name="promote-to-domain-controller"></a>升级到域控制器  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Promote.png)  
  
在 AD DS 角色安装结束时，你可以使用“将此服务器升级为域控制器”  链接继续配置。 使服务器成为域控制器需要此操作，但不需要立即运行配置向导。 例如，你可能仅希望使用 AD DS 二进制文件设置服务器，然后将它们发送另一个分支机构以进行以后的配置。 通过在传送前添加 AD DS 角色，可以在它到达其目的地时节省时间。 你还可以遵循不使域控制器连续脱机几天或几周的最佳实践。 最后，这使你可以在域控制器升级前更新组件，可为你节省至少一次后续的重新启动操作。  
  
稍后选择此链接将调用 ADDSDeployment cmdlet： **install-addsforest**、 **install-addsdomain**或 **install-addsdomaincontroller**。  
  
### <a name="uninstallingdisabling"></a>卸载/禁用  
无论是否已将服务器升级到域控制器，你都可以按照和其他角色相同的方法删除 AD DS 角色。 但是，删除 AD DS 角色需要在完成时重新启动。  
  
Active Directory 域服务角色删除与安装不同，因为它在完成之前需要域控制器降级。 非常有必要阻止域控制器在未在林中正确清理元数据的情况下卸载其角色二进制文件。 有关详细信息，请参阅[降级域控制器和域&#40;级别 200&#41;](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md)。  
  
> [!WARNING]  
> 不支持在升级到域控制器后使用 Dism.exe 或 Windows PowerShell DISM 模块删除 AD DS 角色，并且将阻止服务器正常启动。  
>   
> 与服务器管理器或 Windows PowerShell 的 AD DS 部署模块不同，DISM 是一种本机服务系统，它本身并不知道 AD DS 或其配置。 不要使用 Dism.exe 或 Windows PowerShell DISM 模块卸载 AD DS 角色，除非服务器不再是域控制器。  
  
### <a name="create-an-ad-ds-forest-root-domain-with-server-manager"></a>使用服务器管理器创建一个 AD DS 目录林根级域  
下图说明了 Active Directory 域服务配置过程，在此案例中你之前已安装 AD DS 角色并使用服务器启动了“Active Directory 域服务配置向导”  。  
  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_forestdeploy2.png)  
  
#### <a name="deployment-configuration"></a>部署配置  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddNewForest.png)  
  
服务器管理器从“部署配置”  页开始进行每个域控制器升级。 其余选项和必填字段在此页面和后续页面上会有所变化，这视所选部署操作而定。  
  
若要创建新的 Active Directory 林，请单击“添加新林” 。 你必须提供有效的根域名：该名称不能为单标记（例如，该名称必须是 *contoso.com* 或类似名称，而不只是 *contoso*），而且必须使用允许的 DNS 域命名要求。  
  
有关有效域名的详细信息，请参阅知识库文章 [Active Directory 中计算机、域、站点和 OU 的命名约定](https://support.microsoft.com/kb/909264)。  
  
> [!WARNING]  
> 不要使用与外部 DNS 名称相同的名称创建新的 Active Directory 林。 例如，如果你的 Internet DNS URL 是 http://contoso.com，必须选择其他名称为内部林以避免将来的兼容性问题。 此名称应该是唯一的且不会产生 Web 流量。 例如：corp.contoso.com。  
  
新的林不需要用于域管理员帐户的新凭据。 域控制器升级进程使用内置管理员帐户的凭据，该帐户来自用于创建目录林根的第一个域控制器。 没有任何方法（默认情况下）可禁用或排除内置管理员帐户，如果其他管理域帐户不可用，它可能是林的唯一入口点。 在部署新林前知道密码至关重要。  
  
**DomainName** 需要有效的完全限定的域 DNS 名称，并且这是必需的内容。  
  
#### <a name="domain-controller-options"></a>域控制器选项  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_DCOptions_Forest.gif)  
  
“域控制器选项”使你能够为新的目录林根级域配置“林功能级别”和“域功能级别”。 默认情况下，这些设置是在新目录林根域中的 Windows Server 2012。 Windows Server 2012 林功能级别不提供 Windows Server 2008 R2 林功能级别之上的任何新功能。 Windows Server 2012 域功能级别要求仅为了实现新 Kerberos 设置"始终提供声明"和"使未保护身份验证请求失败。" Windows Server 2012 中的功能级别的主要用途是符合允许的最低操作系统要求的域与域控制器中的参与限制。 换而言之，可以指定 Windows Server 2012 域功能级别仅域控制器运行 Windows Server 2012 的可以托管域。  Windows Server 2012 实现名为的新域控制器标志**DS_WIN8_REQUIRED**中**DSGetDcName**独占方式定位 Windows Server 2012 域控制器的 NetLogon 的函数。 这使你在允许在域控制器上运行哪些操作系统方面可以灵活地选择较为同质或较为异质的林。  
  
有关域控制器位置的详细信息，请参阅 [目录服务功能](https://msdn.microsoft.com/library/ms675900(VS.85).aspx)。  
  
唯一的可配置域控制器功能是 DNS 服务器选项。 Microsoft 建议所有域控制器都提供 DNS 服务以实现分布式环境中的高可用性，这是在任何模式下或域中安装域控制器时默认选择此选项的原因。 全局编录和只读域控制器选项在创建新的目录林根级域时不可用；第一个域控制器必须是 GC，而且不能是只读域控制器 (RODC)。  
  
指定的“目录服务还原模式密码”必须遵守应用于服务器的密码策略，该策略在默认情况下不要求强密码；仅需非空密码。 总是选择复杂强密码或首选密码。  
  
#### <a name="dns-options-and-dns-delegation-credentials"></a>DNS 选项和 DNS 委派凭据  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestDNSOptions.png)  
  
“DNS 选项”  页面使你可以配置 DNS 委派，并提供备用 DNS 管理凭据。  
  
安装新的 Active Directory 目录林根级域（其中你在“域控制器选项”页面上选择了“DNS 服务器”）时，你无法在 Active Directory 域服务配置向导中配置 DNS 选项或委派。 在现有的 DNS 服务器基础结构中创建新的目录林根级 DNS 区域时，“创建 DNS 委派”  选项可用。 此选项使你可以提供有权限更新 DNS 区域的备用 DNS 管理凭据。  
  
有关是否需要创建 DNS 委派的详细信息，请参阅 [了解区域委派](https://technet.microsoft.com/library/cc771640.aspx)。  
  
#### <a name="additional-options"></a>其他选项  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestAdditionalOptions.png)  
  
“其他选项”页显示该域的 NetBIOS 名称并使你可以重写它。 默认情况下，NetBIOS 域名与“部署配置”  页上提供的完全限定的域名最左边的标签相匹配。 例如，如果你提供了 corp.contoso.com 的完全限定的域名，则默认 NetBIOS 域名是 CORP。  
  
如果名称是 15 个字符或更少且与另一个 NetBIOS 名称不冲突，则它保持不变。 如果它与另一个 NetBIOS 名称发生冲突，则将数字附加到该名称。 如果该名称多于 15 个字符，则向导提供一个唯一的截断的建议名称。 在任一情况下，向导首先将通过 WINS 查找和 NetBIOS 广播验证该名称尚未被使用。  
  
有关有效域名的详细信息，请参阅知识库文章 [Active Directory 中计算机、域、站点和 OU 的命名约定](https://support.microsoft.com/kb/909264)。  
  
#### <a name="paths"></a>路径  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestPaths.png)  
  
“路径”页可以用于覆盖 AD DS 数据库、数据库事务日志和 SYSVOL 共享的默认文件夹位置。 默认位置始终位于 %systemroot%（即 C:\Windows）的子目录中。  
  
#### <a name="review-options-and-view-script"></a>审查选项和查看脚本  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestReviewOptions.png)  
  
“审查选项”  页面使你可以在开始安装前验证设置并确保它们符合你的要求。 在使用服务器管理器时，这并不是停止安装的最后机会。 这只是一个在继续配置前确认设置的选项  
  
服务器管理器中的“审查选项”  页还提供可选的“查看脚本”  按钮，可将包含当前 ADDSDeployment 配置的 Unicode 文本文件创建为一个 Windows PowerShell 脚本。 这样，你可以将服务器管理器图形界面用作 Windows PowerShell 部署工作台。 使用 Active Directory 域服务配置向导来配置选项、导出配置和取消向导。 此过程将创建有效且语法正确的示例，以便做进一步修改或直接使用。 例如：  
  
```powershell 
#  
# Windows PowerShell Script for AD DS Deployment  
#  
  
Import-Module ADDSDeployment  
Install-ADDSForest `  
-CreateDNSDelegation `  
-DatabasePath "C:\Windows\NTDS" `  
-DomainMode "Win2012" `  
-DomainName "corp.contoso.com" `  
-DomainNetBIOSName "CORP" `  
-ForestMode "Win2012" `  
-InstallDNS:$true `  
-LogPath "C:\Windows\NTDS" `  
-NoRebootOnCompletion:$false `  
-SYSVOLPath "C:\Windows\SYSVOL"  
-Force:$true  
  
```  
  
> [!NOTE]  
> 服务器管理器通常在升级时使用值填充所有参数，并且不依赖默认值（因为它们可能在 Windows 或服务包以后的版本之间发生更改）。 但该情况的一个例外是 **-safemodeadministratorpassword** 参数（有意从脚本中省略）。 若要强制执行确认提示，则在以交互方式运行 cmdlet 时省略该值。  
  
#### <a name="prerequisites-check"></a>先决条件检查  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestPrereqCheck.png)  
  
“先决条件检查”  是 AD DS 域配置中的新功能。 此新阶段验证服务器配置是否能够支持新的 AD DS 林。  
  
在安装新目录林根级域时，服务器管理器 Active Directory 域服务配置向导将调用一系列的模块化测试。 这些测试向你提出警告并提供建议的修复选项。 你可以根据需要多次运行测试。 域控制器进程在所有先决条件测试通过前无法继续。  
  
“先决条件检查”还显示相关的信息，例如影响较早版本的操作系统的安全性更改。  
  
有关特定先决条件检查的详细信息，请参阅 [Prerequisite Checking](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking)。  
  
#### <a name="installation"></a>安装  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestInstallation.png)  
  
当“安装”  页显示时，域控制器配置将开始，并且无法暂停或取消。 详细操作显示在此页面上并将写入日志：  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
> [!NOTE]  
> 你可以从同一个服务器管理器控制台同时运行多个角色安装和 AD DS 配置向导。  
  
#### <a name="results"></a>结果  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
“结果”  页面显示升级是成功还是失败以及任何重要的管理信息。 域控制器将在 10 秒后自动重新启动。  
  
## <a name="BKMK_PSForest"></a>部署具有 Windows PowerShell 的林  
本部分介绍如何在核心 Windows Server 2012 计算机上使用 Windows PowerShell 在目录林根级域中安装第一个域控制器。  
  
### <a name="windows-powershell-ad-ds-role-installation-process"></a>Windows PowerShell AD DS 角色安装过程  
通过将一些简单的 ServerManager 部署 cmdlet 实现到部署过程，你进一步实现了 AD DS 简化管理的愿景。  
  
下一张图阐释了 Active Directory 域服务角色安装过程，以你运行 **PowerShell.exe** 开始，在升级域控制器前结束。  
  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_servermanagerdeployment_powershell.png)  
  
|||  
|-|-|  
|ServerManager Cmdlet|参数（需要**加粗**参数。 *斜体*参数可以通过使用 Windows PowerShell 或 AD DS 配置向导来指定。）|  
|Install-WindowsFeature/Add-WindowsFeature|***-Name***<br /><br />*-Restart*<br /><br />*-IncludeAllSubFeature*<br /><br />*-IncludeManagementTools*<br /><br />-Source<br /><br />*-ComputerName*<br /><br />-Credential<br /><br />-LogPath<br /><br />*-Vhd*<br /><br />*-ConfigurationFilePath*|  
  
> [!NOTE]  
> 尽管不是必需的，但在安装 AD DS 角色二进制文件时强烈建议使用 **-IncludeManagementTools** 。  
  
ServerManager 模块公开 Windows PowerShellnew 新 DISM 模块的角色安装、状态和删除部分。 此分层简化了大部分任务并减少了对直接使用强大（但是错误使用时很危险）的 DISM 模块的需求。  
  
使用 **Get-Command** 导出 ServerManager 中的别名和 cmdlet。  
  
```powershell  
Get-Command -module ServerManager  
```  
  
例如：  
  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSGetCommand.png)  
  
若要添加 Active Directory 域服务角色，只需运行 **Install-WindowsFeature** 并将 AD DS 角色名称作为参数。 和服务器管理器一样，将自动安装 AD DS 角色的所有隐式必需服务。  
  
```powershell  
Install-WindowsFeature -name AD-Domain-Services  
```  
  
如果你还希望安装 AD DS 管理工具（强烈建议），请提供 **-IncludeManagementTools** 参数：  
  
```powershell  
Install-WindowsFeature -name AD-Domain-Services -IncludeManagementTools  
```  
  
例如：  
  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallWinFeature.png)  
  
若要列出带有安装状态的所有功能和角色，请使用不带参数的 **Get-WindowsFeature** 参数。 从远程服务器为安装状态指定 **-ComputerName** 参数。  
  
```powershell  
Get-WindowsFeature  
```  
  
由于 **Get-WindowsFeature** 没有筛选机制，必须使用带有管道的 **Where-Object** 查找特定功能。 管道是在多个 cmdlet 之间用来传递数据的通道，Where-Object cmdlet 将充当筛选器。 内置 **$_** 变量充当通过管道的当前对象，并带有任何它可能包含的属性。  
  
```powershell  
Get-WindowsFeature | where-object <options>  
```  
  
例如，若要查找在 **显示名称** 属性中包含“Active Dir”的所有功能，请使用：  
  
```powershell  
Get-WindowsFeature | where displayname -like "*active dir*"  
```  
  
下面所示的其他示例：  
  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSGetWindowsFeature.png)  
  
有关使用管道和 Where-Object 的更多 Windows PowerShell 操作的详细信息，请参阅 [Windows PowerShell 中的管道系统和管道](https://technet.microsoft.com/library/ee176927.aspx)。  
  
另请注意，Windows PowerShell 3.0 大大简化了此管道操作中需要的命令行参数。 Windows PowerShell 2.0 将需要：  
  
```powershell  
Get-WindowsFeature | where {$_.displayname - like "*active dir*"}  
```  
  
通过使用 Windows PowerShell 管道，你可以创建可读的结果。 例如：  
  
```powershell  
Install-WindowsFeature | Format-List  
Install-WindowsFeature | select-object | Format-List  
  
```  
  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDS.png)  
  
请注意，使用带有 **-expandproperty** 参数的 **Select-Object** cmdlet 如何返回有趣的数据：  
  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDSWithTools.png)  
  
> [!NOTE]  
> **Select-Object -expandproperty** 参数会稍稍减慢总体安装性能。  
  
### <a name="BKMK_PS"></a>使用 Windows PowerShell 创建 AD DS 目录林根级域  
若要使用 ADDSDeployment 模块安装新的 Active Directory 林，请使用以下 cmdlet：  
  
```powershell  
Install-addsforest  
```  
  
**Install-AddsForest** cmdlet 仅有两个阶段（先决条件检查和安装）。 下面两个图片显示带有最少所需参数 **-domainname**的安装阶段。  
  
|||  
|-|-|  
|ADDSDeployment Cmdlet|参数（需要**加粗**参数。 *斜体*参数可以通过使用 Windows PowerShell 或 AD DS 配置向导来指定。）|  
|install-addsforest|-Confirm<br /><br />*-CreateDNSDelegation*<br /><br />*-DatabasePath*<br /><br />*-DomainMode*<br /><br />***-DomainName***<br /><br />***-DomainNetBIOSName***<br /><br />*-DNSDelegationCredential*<br /><br />*-ForestMode*<br /><br />-Force<br /><br />*-InstallDNS*<br /><br />*-LogPath*<br /><br />-NoDnsOnNetwork<br /><br />-NoRebootOnCompletion<br /><br />*-SafeModeAdministratorPassword*<br /><br />-SkipAutoConfigureDNS<br /><br />-SkipPreChecks<br /><br />*-SYSVOLPath*<br /><br />*-Whatif*|  
  
> [!NOTE]  
> 如果要更改根据 DNS 域名前缀自动生成的 15 字符名称或名称超过 15 个字符，需要 **-DomainNetBIOSName** 参数。  
  
等效的服务器管理器 **部署配置** ADDSDeployment cmdlet 和参数是：  
  
```powershell  
Install-ADDSForest  
-DomainName <string>  
```  
  
等效的服务器管理器域控制器选项 ADDSDeployment cmdlet 参数是：  
  
```powershell  
-ForestMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>  
-DomainMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>  
-InstallDNS <{$false | $true}>  
-SafeModeAdministratorPassword <secure string>  
  
```  
  
**Install-ADDSForest** 参数使用与服务器管理器相同的默认值（如果未指定）。  
  
**SafeModeAdministratorPassword** 参数的操作是特殊操作：  
  
-   如果 *未指定* 为参数，cmdlet 将提示你输入并确认掩蔽密码。 以交互方式运行 cmdlet 时，这是首选用法。  
  
    例如，若要创建名为 corp.contoso.com 的新林，并且收到输入并确认掩蔽密码的提示：  
  
    ```powershell  
    Install-ADDSForest "DomainName corp.contoso.com  
    ```  
  
-   如果指定*值*，则该值必须是安全字符串。 以交互方式运行 cmdlet 时，这不是首选用法。  
  
例如，可通过使用 **Read-Host** cmdlet 提示用户提供安全字符串来手动提示输入密码：  
  
```powershell  
-safemodeadministratorpassword (read-host -prompt "Password:" -assecurestring)  
```  
  
> [!WARNING]  
> 由于上个选项未确认密码，请务必十分谨慎：密码不可见。  
  
你还可以提供安全字符串作为转换的明文变量，尽管强烈不建议这样做。  
  
```powershell  
-safemodeadministratorpassword (convertto-securestring "Password1" -asplaintext -force)  
```  
  
最后，可以将模糊的密码存储在文件中，然后在以后重复使用它，而不会出现明文密码。 例如：  
  
```powershell  
$file = "c:\pw.txt"  
$pw = read-host -prompt "Password:" -assecurestring  
$pw | ConvertFrom-SecureString | Set-Content $file  
  
-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)  
  
```  
  
> [!WARNING]  
> 不建议提供或存储清晰或模糊的文本密码。 任何在脚本中运行此命令或看到的人员都知道该域控制器的 DSRM 密码。 任何有权限访问该文件的人都可反转该模糊密码。 知道密码后，他们可以登录到以 DSRM 模式启动的 DC 并最终模拟域控制器本身，从而将他们的权限提升到 Active Directory 林中的最高级。 使用 **System.Security.Cryptography** 加密文本文件数据的一组其他步骤是可取的，但不在范围内。 最佳实践是完全避免密码存储。  
  
ADDSDeployment cmdlet 提供其他选项以跳过 DNS 客户端设置、转发器和根提示的自动配置。 使用服务器管理器时，不能跳过此配置选项。 仅当已在配置域控制器之前安装 DNS 服务器角色时，此参数才十分重要：  
  
```powershell  
-SkipAutoConfigureDNS  
```  
  
**DomainNetBIOSName** 操作是特殊操作：  
  
-   如果未使用 NetBIOS 域名指定 **DomainNetBIOSName** 参数且 **DomainName** 参数中的单标签前缀域名等于或少于 15 个字符，则升级使用自动生成的名称继续。  
  
-   如果未使用 NetBIOS 域名指定 **DomainNetBIOSName** 参数且 **DomainName** 参数中的单标签前缀域名等于或多于 16 字符，则升级失败。  
  
-   如果使用等于或少于 15 个字符的 NetBIOS 域名指定 **DomainNetBIOSName** 参数，则升级将使用该指定的名称继续。  
  
-   如果使用等于或多于 16 个字符的 NetBIOS 域名指定 **DomainNetBIOSName** 参数，则升级失败。  
  
等效的服务器管理器其他选项 ADDSDeployment cmdlet 参数是：  
  
```powershell  
-domainnetbiosname <string>  
```  
  
等效的服务器管理器 **路径** ADDSDeployment cmdlet 参数是：  
  
```powershell  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
  
```  
  
将可选的 **Whatif** 参数与 **Install-ADDSForest** cmdlet 一起使用以查看配置信息。 这使你可以查看 cmdlet 的参数的显式值和隐式值。  
  
例如：  
  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSPaths.png)  
  
使用服务器管理器时你无法绕过“先决条件检查”  ，但是你可以在使用 AD DS 部署 cmdlet 时使用以下参数跳过该进程：  
  
```powershell  
-skipprechecks  
```  
  
> [!WARNING]  
> Microsoft 不鼓励跳过先决条件检查，因为它可能导致部分域控制器升级或 AD DS 林损坏。  
  
请注意 **Install-ADDSForest** 如何提醒你升级将自动重新启动服务器，就像服务器管理器一样。  
  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSReboot.png)  
  
![安装一个新林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallProgress.png)  
  
若要自动接受重新启动提示，请将 **-force** 或 **-confirm:$false** 参数与任何 ADDSDeployment Windows PowerShell cmdlet 一起使用。 若要防止服务器在升级结束时自动重新启动，请使用 **-norebootoncompletion** 参数。  
  
> [!WARNING]  
> 不建议重写重新启动。 域控制器必须重新启动才能正常工作。  
  
## <a name="see-also"></a>请参阅  
[Active Directory 域服务 （TechNet 门户）](https://technet.microsoft.com/library/cc770946(WS.10).aspx)  
[适用于 Windows Server 2008 R2 的 active Directory 域服务](https://technet.microsoft.com/library/dd378801(WS.10).aspx)  
[适用于 Windows Server 2008 的 active Directory 域服务](https://technet.microsoft.com/library/dd378891(WS.10).aspx)  
[Windows Server 技术参考 (Windows Server 2003)](https://technet.microsoft.com/library/cc739127(WS.10).aspx)  
[Active Directory 管理中心：入门 (Windows Server 2008 R2)](https://technet.microsoft.com/library/dd560651(WS.10).aspx)  
[使用 Windows PowerShell (Windows Server 2008 R2) 的 active Directory 管理](https://technet.microsoft.com/library/dd378937(WS.10).aspx)  
[询问目录服务团队 （官方 Microsoft 商业技术支持博客）](http://blogs.technet.com/b/askds)  
  

