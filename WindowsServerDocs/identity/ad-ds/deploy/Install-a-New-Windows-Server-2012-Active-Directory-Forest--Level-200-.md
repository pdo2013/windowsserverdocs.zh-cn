---
ms.assetid: b3d6fb87-c4d4-451c-b3de-a53d2402d295
title: "安装新的 Windows Server 2012 的 Active Directory 森林 (级别 200)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b8a7502a1b9d27b0f61353f2544182a64d311496
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="install-a-new-windows-server-2012-active-directory-forest-level-200"></a>安装新的 Windows Server 2012 的 Active Directory 森林 (级别 200)

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本主题介绍了新的 Windows Server 2012 Active Directory 域服务域控制器推广功能介绍性级别。 在 Windows Server 2012、广告 DS 替换 Dcpromo 工具使用服务器管理器和 Windows PowerShell 基于部署系统。  
  
-   [Active Directory 域服务简化的管理](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SimplifiedAdmin)  
  
-   [Technical 概述](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_TechOverview)  
  
-   [部署森林使用服务器管理器](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SMForest)  
  
-   [部署 Windows PowerShell 与森林](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_PSForest)  
  
## <a name="BKMK_SimplifiedAdmin"></a>Active Directory 域服务简化的管理  
Windows Server 2012 引入新一代的 Active Directory 域服务简体中文管理，且最字根域重新构想自 Windows 2000 Server。 广告 DS 简体中文管理捕获的 Active Directory 的 12 年中经验，并使更多支持、更灵活、更直观的管理体验设计师和管理员。 这意味着创建新版本的现有的技术，以及扩展组件发布了 Windows Server 2008 R2 的能力。  
  
### <a name="what-is-ad-ds-simplified-administration"></a>什么是广告 DS 简体中文管理？  
广告 DS 简体中文管理是 reimagining 的域部署。 一些这些功能包括：  
  
-   广告 DS 角色部署现在位于新服务器管理器的体系结构并允许远程安装。  
  
-   广告 DS 部署和配置引擎现在是 Windows PowerShell，即使使用图形设置。  
  
-   推广现在包含先决条件检查验证林和域准备新的域控制器，降低失败促销的机会。  
  
-   Windows Server 2012 树林级别不实现新功能，并且仅的新 Kerberos 功能，免除频繁的管理员子集需要域功能级别功能需要同类域控制器环境。  
  
### <a name="purpose-and-benefits"></a>目的和享有其利益  
这些更改可能会出现更复杂、不更易于使用。 尽管重新设计广告 DS 部署过程，在没有机会合并到较少的、更简单的操作的许多步骤和最佳做法。 例如，这意味着新副本域控制器图形配置现在是八对话，而不是之前的 12。 创建新的 Active Directory 森林需要*单个*仅用的 Windows PowerShell 命令*一个*参数：的域的名称。  
  
为什么是否有 Windows PowerShell 在 Windows Server 2012 上的此类强调？ 分布式计算发展，如 Windows PowerShell 允许有一个引擎配置和维护同时图形从命令行界面。 它将允许面向 IT 专业人员，以供开发人员授予 API 齐全脚本相同第一个类公民与任何组件。 基于云的计算普遍随着，Windows PowerShell 还最后带来远程管理的服务器，在没有图形接口的计算机具有相同的管理功能正团结一心监视器鼠标与的能力。  
  
经验丰富的广告 DS 管理员应该会发现他们以前知识高度相关。 开始管理员将查找远平坦学习。  
  
## <a name="BKMK_TechOverview"></a>Technical 概述  
  
### <a name="what-you-should-know-before-you-begin"></a>什么在开始之前，你应了解  
本主题假定熟悉的 Active Directory 域服务，以前的版本，并不提供基础周围其用途和功能的详细信息。 有关广告 DS 的详细信息，请参阅 TechNet 门户页面链接如下：  
  
-   [Windows Server 2008 r2 的 Active Directory 域服务](https://technet.microsoft.com/library/dd378801(WS.10).aspx)  
  
-   [适用于 Windows Server 2008 Active Directory 域服务](https://technet.microsoft.com/library/dd378891(WS.10).aspx)  
  
-   [Windows Server 技术参考](https://technet.microsoft.com/library/cc739127(WS.10).aspx)  
  
### <a name="functional-descriptions"></a>功能的说明  
  
#### <a name="ad-ds-role-installation"></a>广告 DS 角色安装  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_SelectServerRoles.gif)  
  
Active Directory 域服务安装当作服务器管理器和 Windows PowerShell，所有其他 server 角色和 Windows Server 2012 的功能。 Dcpromo.exe 计划不再提供 GUI 配置选项。  
  
你为 Windows PowerShell 本地和远程安装中使用的图形向导中服务器管理器或 ServerManager 模块。 通过运行多个这些向导或 cmdlet 实例，并针对不同的服务器，你可以将部署广告 DS 到多个域控制器同时，所有从一个单独的控制台。 尽管这些新功能不与 Windows Server 2008 R2 或更早的操作系统向后兼容，你也仍然可以使用 Dism.exe 应用程序引入用于本地角色安装从命令行经典 Windows Server 2008 R2。  
  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSAddWindowsFeature.png)  
  
#### <a name="ad-ds-role-configuration"></a>广告 DS 角色配置  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_DeploymentConfiguration_Forest.gif)  
  
"以前称为 DCPROMO"Active Directory 域服务配置是现在从角色安装单独的操作。 安装后广告 DS 角色，管理员身份配置为使用一个单独的向导中服务器管理器，或使用 ADDSDeployment Windows PowerShell 模块的域控制器服务器。  
  
广告 DS 角色配置字段体验的 12 年中的版本，并现在配置域控制器根据 Microsoft 的最新的最佳实践。 例如，域名系统和全球目录安装每个域控制器上的默认情况下。  
  
服务器管理器广告 DS 配置向导将许多单独的对话合并到较少的提示，并不会再隐藏在"高级"模式下的设置。 整个升级过程中在安装期间是一个扩展的对话框。 该向导，ADDSDeployment Windows PowerShell 模块显示你明显的更改并提供指向详细信息的安全问题。  
  
Dcpromo.exe 命令行参与安装仅，将保留在 Windows Server 2012 并且不会再运行图形安装向导。 强烈建议你不再无人照看安装 Dcpromo.exe 用于，并将其替换为 ADDSDeployment 模块，如现在弃用可执行文件不会包含在 Windows 中的下一个版本。  
  
这些新功能不会对 Windows Server 2008 R2 或较旧操作系统向后兼容。  
  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDSForest.png)  
  
> [!IMPORTANT]  
> Dcpromo.exe 不会再包含图形向导，并且不会再安装角色或功能二进制文件。 尝试运行从资源管理器 shell 返回 Dcpromo.exe:  
>   
> "Active Directory 域服务安装向导已重新定位中服务器管理器。 有关详细信息，请参阅 https://go.microsoft.com/fwlink/?LinkId=220921。"  
>   
> 尝试运行 Dcpromo.exe//unattend 仍然安装了二进制文件，如下所示以前的操作系统，但警告：  
>   
> "Dcpromo 无人照看的操作将为 Windows PowerShell 替换 ADDSDeployment 模块。 有关详细信息，请参阅 https://go.microsoft.com/fwlink/?LinkId=220924。"  
>   
> Windows Server 2012 成为 dcpromo.exe 和它不会包含在将来版本的 Windows，也不将其收到进一步增强功能在此操作系统。 管理员应该不再使用它，并切换到受支持的 Windows PowerShell 模块，如果他们希望从命令行中创建域控制器。  
  
#### <a name="prerequisite-checking"></a>先决条件检查  
此外实现了域控制器配置评估森林和前继续使用域控制器升级的域的先决条件检查阶段。 这包括 FSMO 角色可用性、用户特权、扩展的架构兼容性和其他要求。 这一新设计缓解了域控制器升级其中启动，然后暂停中间致命配置错误的问题。 这减少孤立的域控制器森林中的元数据的机会或不正确认为服务器域控制器。  
  
## <a name="BKMK_SMForest"></a>部署森林使用服务器管理器  
此部分中介绍了如何在森林根域图形 Windows Server 2012 计算机上使用服务器管理器中安装的第一个域控制器。  
  
### <a name="server-manager-ad-ds-role-installation-process"></a>服务器管理器广告 DS 角色安装过程  
下图所示的 Active Directory 域服务角色安装进程，开头你运行的 ServerManager.exe 和的域控制器在升级前直接结束。  
  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_servermanagerdeployment.png)  
  
#### <a name="server-pool-and-add-roles"></a>服务器池，然后添加角色  
从计算机运行的服务器管理器中访问任何 Windows Server 2012 的计算机有资格池。 一旦汇集在一起，你会选择远程安装广告 DS 或任何其他配置选项可能服务器管理器内的这些服务器。  
  
若要添加的服务器，请选择下列情况之一：  
  
-   单击**添加其他服务器管理**仪表板欢迎磁贴  
  
-   单击**管理**菜单并选择**添加服务器**  
  
-   右键单击**所有服务器**选择**添加服务器**  
  
这将打开添加服务器对话框：  
  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddServers.png)  
  
这将为你提供添加到使用或分组池服务器三种方法：  
  
-   搜索 Active Directory（使用 LDAP，需要计算机属于某个域，允许操作系统筛选和支持通配符）  
  
-   搜索 DNS（使用 DNS 别名或通过 ARP 或 NetBIOS 广播或 WINS 查找 IP 地址不允许操作系统筛选或支持通配符）  
  
-   导入（使用回车月换行的分隔服务器的文本文件列表）。  
  
单击**现在查找**要从计算机已连接到该相同的 Active Directory 域返回服务器的列表，请单击一个或多个服务器姓名，从列表中服务器。 单击要添加到服务器向右箭头**选定**列表。 使用**添加服务器**仪表板角色组向添加所选的服务器的对话框。 或单击**管理**，然后单击**创建服务器组**，或单击**创建服务器组**仪表板上**欢迎到服务器管理器**创建自定义服务器组的磁贴。  
  
> [!NOTE]  
> 添加服务器过程不验证在线或访问有服务器。 但是，不能访问的任何服务器标记可管理性视图中服务器管理器中的下一个恢复  
  
你可以安装角色远程任何 Windows Server 2012 上计算机添加池，如下所示：  
  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/tADDS_SMI_TR_AddRolesFeatures.png)  
  
你完全不能管理运行 Windows Server 2012 比较旧操作系统的服务器。 **添加角色和功能**选择运行的 ServerManager Windows PowerShell 模块**安装 WindowsFeature**。  
  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddADDSToAnotherServer.png)  
  
此外可以使用现有的域控制器上服务器管理器仪表板具有右键单击广告 DS 仪表板磁贴，然后依次选择已事先选择角色选择远程服务器广告 DS 安装**添加到另一台服务器广告 DS**。 这调用**安装 WindowsFeature 广告域服务**。  
  
你运行的服务器管理器在计算机池自动本身。 若要安装的广告 DS 角色，只需单击**管理**菜单，然后单击**添加角色和功能**。  
  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ManageAddRoles.png)  
  
#### <a name="installation-type"></a>安装类型  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectInstallationType.png)  
  
**安装类型**对话框提供一个不支持 Active Directory 域服务的选项：**远程桌面服务方案基于安装**。 该选项仅在多个服务器分布式工作负载允许远程桌面服务。 如果你选择它，广告 DS 无法安装。  
  
始终将默认选择保留在原处安装广告 DS 时：**角色基于或功能的安装**。  
  
#### <a name="server-selection"></a>选择服务器  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectDestinationServer.png)  
  
**选择服务器**对话框，您可以选择从一台服务器之前将其添加到池，只要它是访问。 本地服务器运行服务器管理器是自动可用。  
  
此外，你可以选择与 Windows Server 2012 操作系统脱机 Hyper-V VHD 文件并服务器管理器组件服务通过直接向其添加角色。 这允许你在进一步配置它们之前预配有必要的组件虚拟服务器。  
  
#### <a name="server-roles-and-features"></a>服务器角色和功能  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectServerRoles.png)  
  
选择**Active Directory 域服务**如果你打算提升域控制器角色。 所有 Active Directory 管理功能和服务需要自动安装，即使它们表面上的另一个角色一部分或界面服务器管理器中不显示所选。  
  
Server 管理器中还提供了显示此角色隐式安装; 哪些管理功能信息性对话框这相当于**-IncludeManagementTools**参数。  
  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddFeaturesDialog.gif)  
  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectFeatures.png)  
  
其他**功能**可以在此处添加所需。  
  
#### <a name="active-directory-domain-services"></a>Active Directory 域服务  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ADDSIntro.png)  
  
**Active Directory 域服务**对话框要求和最佳做法上提供的信息有限。 主要作为所选 AD DS 角色确认"未显示此屏幕，如果您没有选择广告 DS。  
  
#### <a name="confirmation"></a>确认  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Confirmation.png)  
  
**确认**开始角色安装之前有最终检查点对话框。 它提供一个选项来根据角色安装后，需要重启计算机，但广告 DS 安装不需要重新启动。  
  
通过单击**安装**，确认准备好开始角色安装。 一旦开始后，你无法取消角色安装。  
  
#### <a name="results"></a>结果  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Results.png)  
  
**结果**对话框中显示当前的安装进度和当前安装的状态。 无论是否关闭服务器管理器会继续角色安装。  
  
验证安装结果仍是最佳实践。 如果你关闭**结果**对话框之前完成安装后，你可以检查使用 Server 管理器中通知标志的结果。 此外将 server 管理器中显示任何已安装的广告 DS 角色，但尚未进一步配置为域控制器服务器一条警告的消息。  
  
**任务的通知**  
  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_TaskNotofications.png)  
  
**广告 DS 详细信息**  
  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ADDSDetails.png)  
  
**任务的详细信息**  
  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_TaskDetails.png)  
  
#### <a name="promote-to-domain-controller"></a>推广域控制器  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Promote.png)  
  
在安装广告 DS 角色结束时，你可以继续使用配置使用**推广此域控制器服务器**链接。 这使服务器域控制器，所需，但不需要立即运行配置向导。 例如，你可能只想发送到更高版本配置为另一台分支机构之前配置服务器的广告 DS 二进制文件。 通过添加 AD DS 角色发货之前，你节省使用时间何时到达目的地。 你还可以按照未保持域控制器离线，用于天或数周的最佳实践。 最后，这使你组件域控制器升级，保存你在至少有一个后续重新启动之前的更新。  
  
选择该链接稍后调用 ADDSDeployment cmdlet:**安装 addsforest**，**安装 addsdomain**，或**安装 addsdomaincontroller**。  
  
### <a name="uninstallingdisabling"></a>卸载禁用  
删除广告 DS 角色其他角色，无论你在升级到某个域控制器服务器是否等。 但是，删除广告 DS 角色，需要在完成重启。  
  
不同安装、Active Directory 域服务角色删除是，它可以完成之前，它需要域控制器降级。 这是必需以防止域控制器卸载不正确的元数据清理森林中的角色二进制文件。 有关详细信息，请参阅[降级域控制器和域 & #40;级别 200 & #41;](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
> [!WARNING]  
> 升级到某个域控制器不支持，这将阻止服务器正常启动后，请删除 Dism.exe 或 DISM Windows PowerShell 模块的广告 DS 角色。  
>   
> 像服务器管理器或广告 DS 部署为 Windows PowerShell 模块，DISM 是原始 servicing 系统具有广告 DS 没有本身知道或其配置。 不要使用 Dism.exe 或 DISM Windows PowerShell 模块除非服务器不再域控制器卸载广告 DS 角色。  
  
### <a name="create-an-ad-ds-forest-root-domain-with-server-manager"></a>创建一个广告 DS 森林根域使用服务器管理器  
下图说明 Active Directory 域服务配置过程中的，在这种情况，你有以前安装广告 DS 角色并开始**Active Directory 域服务配置向导**使用服务器管理器。  
  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_forestdeploy2.png)  
  
#### <a name="deployment-configuration"></a>部署配置  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddNewForest.png)  
  
服务器管理器开始与每域控制器升级**部署配置**页面。 其他选项和必填的字段在此页和后续页面，具体取决于哪个部署操作你选择的更改。  
  
若要创建新的 Active Directory 森林，请单击**添加新的林**。 你必须提供有效的根域名;无法单标记名称 (例如，必须名称*contoso.com*或类似并不只是*contoso*)，必须使用允许的 DNS 域命名要求。  
  
有关详细信息有效域名，请参阅知识库文章[的计算机、域、网站和华丽绚烂命名 Active Directory 中的约定](https://support.microsoft.com/kb/909264)。  
  
> [!WARNING]  
> 不要与外部 DNS 具有相同名称中创建新的 Active Directory 森林。 例如，如果你的 Internet DNS URL http://contoso.com，你必须选择另一个名称内部林以避免未来兼容性问题。 该名称应该唯一和确信网站的交通。 例如：corp.contoso.com。  
  
新林不需要新的凭据域的管理员帐户。 域控制器升级过程中会使用管理员帐户从第一个域控制器用于创建森林根内置的凭据。 没有任何方法（默认）来禁用或锁定内置的管理员帐户，如果是不可用的其他管理域帐户，可能森林仅入口点。 部署新林之前知道密码至关重要。  
  
**域名**需要有效的完整的域 DNS 名称，并且需要。  
  
#### <a name="domain-controller-options"></a>域控制器选项  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_DCOptions_Forest.gif)  
  
**域控制器选项**使您能够配置**林功能级别**和**域功能级别**新建森林根域。 默认情况下，这些设置均新林根域中的 Windows Server 2012。 在 Windows Server 2008 R2 森林功能级别的 Windows Server 2012 森林功能级别不提供任何新的功能。 Windows Server 2012 域功能级别需要仅以实现新 Kerberos 设置"始终提供索赔"和"无法 unarmored 身份验证的请求。" 在 Windows Server 2012 功能级别主要用于是限制参与满足最低允许操作系统要求到域控制器。 换言之，你可以指定 Windows Server 2012 功能级别仅域控制器域运行 Windows Server 2012 的可以容纳域。  Windows Server 2012 实现名为一个新的域控制器标志**DS_WIN8_REQUIRED**中**dsgetdcname 发现**独享定位 Windows Server 2012 域控制器的 NetLogon 的函数。 这允许你在该操作系统获准运行域控制器上的更多同类或不同森林的灵活性。  
  
有关域控制器位置的详细信息，请参阅[目录服务功能](https://msdn.microsoft.com/library/ms675900(VS.85).aspx)。  
  
只可配置域控制器功能是 DNS 服务器选项。 Microsoft 建议所有域控制器都提供分布式环境，这正是时安装域控制器在任何模式或域中，选择此选项默认情况下的高可用性 DNS 服务。 全球目录和阅读仅域控制器选项不可用时创建新森林根域;第一个域控制器必须 GC，并且不能为阅读仅域控制器 (RODC)。  
  
指定**目录服务还原模式密码**必须遵守密码策略应用到服务器，默认情况下它不需要使用强密码;仅非空一个。 始终可以选择复杂的强密码或最好，密码。  
  
#### <a name="dns-options-and-dns-delegation-credentials"></a>DNS 选项和 DNS 委派凭据  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestDNSOptions.png)  
  
**DNS 选项**页面使您能够配置 DNS 委派并提供备用 DNS 管理的凭据。  
  
你不能配置 DNS 选项或委派 Active Directory 域服务配置向导中安装所选的其中一个新 Active Directory 森林根域时**DNS 服务器**上**域控制器选项**页面。 **创建 DNS 委派**现有 DNS 服务器基础结构中创建新林根 DNS 区域时，选项才可用。 此选项可使您可以提供有权更新 DNS 区域的备用 DNS 管理凭据。  
  
你需要创建 DNS 委派的详细信息，请参阅[了解区域委派](https://technet.microsoft.com/library/cc771640.aspx)。  
  
#### <a name="additional-options"></a>其他选项  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestAdditionalOptions.png)  
  
**其他选项**页面显示的域 NetBIOS 名称，并使您可以将其重写。 默认情况下，NetBIOS 域名称匹配合法的域名上提供的最左端标签**部署配置**页面。 例如，如果提供 corp.contoso.com 合法的域名，默认 NetBIOS 域名是 CORP.  
  
如果名 15 字符或更少和不冲突用另一台 NetBIOS 名称、它是不变。 如果它会与其他 NetBIOS 名称有冲突，数字附加为的名称。 如果名多 15 字符，该向导将提供独特的、被截断的建议。 不论采取何种情况，该向导首先验证名称尚不在使用通过 WINS 查找和广播 NetBIOS。  
  
有关详细信息有效域名，请参阅知识库文章[的计算机、域、网站和华丽绚烂命名 Active Directory 中的约定](https://support.microsoft.com/kb/909264)。  
  
#### <a name="paths"></a>路径  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestPaths.png)  
  
**路径**页使你可以覆盖默认文件夹位置的广告 DS 数据库数据库事务日志中，并 SYSVOL 共享。 始终处于子目录 %系统根 %(即 C:\Windows) 的默认位置。  
  
#### <a name="review-options-and-view-script"></a>查看选项并查看脚本  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestReviewOptions.png)  
  
**回顾选项**页面，可以让你以验证你的设置并确保它们符合你的要求，开始安装之前。 这不是最后一次机会停止安装时使用服务器管理器。 这是只需继续配置之前，请确认你的设置的选项  
  
**回顾选项**页面中服务器管理器中还提供了一个可选**查看脚本**按钮来创建 Unicode 文本文件包含当前 ADDSDeployment 配置为单个 Windows PowerShell 脚本。 这使你为 Windows PowerShell 部署 studio 使用服务器管理器图形界面。 使用 Active Directory 域服务配置向导配置选项中, 导出配置，然后取消向导。 此过程中创建进一步修改或直接使用有效和语法正确的示例。 例如：  
  
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
> 服务器管理器通常会填满所有参数与值时升级，不依赖于默认设置（如它们可能会更改之间将来版本的 Windows 或 service pack）中。 是一个例外**-safemodeadministratorpassword**参数（这有意省略脚本中）。 若要强制确认提示，请运行 cmdlet 交互时忽略值。  
  
#### <a name="prerequisites-check"></a>先决条件复选  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestPrereqCheck.png)  
  
**先决条件检查**是广告 DS 域配置中的新增功能。 此新阶段验证的服务器配置能够支持新的广告 DS 森林。  
  
当安装新林根域，服务器管理器 Active Directory 域服务配置向导调用一系列的模块化测试。 这些测试向你发出警报建议的维修选项。 你可以根据需要多次运行测试。 域控制器过程所有先决条件测试才可继续通过。  
  
**先决条件检查**还面，如会影响较旧操作系统的安全更新的相关信息。  
  
特定的先决条件检查的详细信息，请参阅[先决条件检查](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking)。  
  
#### <a name="installation"></a>安装  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestInstallation.png)  
  
当**安装**页面显示时，域控制器配置开始，无法将停止或取消。 详细的操作显示在此页面上，并且写入日志：  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
> [!NOTE]  
> 你可以多个角色安装和广告 DS 配置向导从相同服务器管理控制台同时运行。  
  
#### <a name="results"></a>结果  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
**结果**页面显示的成功或失败的升级和管理的任何重要信息。 10 秒钟后，域控制器将自动重新启动。  
  
## <a name="BKMK_PSForest"></a>部署 Windows PowerShell 与森林  
此部分中介绍了如何在 Windows Server 2012 的核心计算机上使用 Windows PowerShell 森林根域中安装的第一个域控制器。  
  
### <a name="windows-powershell-ad-ds-role-installation-process"></a>Windows PowerShell 广告 DS 角色安装过程  
通过到部署进程实施几个简单 ServerManager 部署 cmdlet，您进一步实现的广告 DS 影像简体中文管理。  
  
下图说明 Active Directory 域服务角色安装过程中，你运行的从开始**PowerShell.exe**和的域控制器在升级前直接结束。  
  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_servermanagerdeployment_powershell.png)  
  
|||  
|-|-|  
|ServerManager Cmdlet|参数 (**加粗**参数是必需的。 *斜体*可以通过使用 Windows PowerShell 或广告 DS 配置向导指定参数。)|  
|安装-WindowsFeature/添加-WindowsFeature|***-名称***<br /><br />*重启*<br /><br />*-IncludeAllSubFeature*<br /><br />*-IncludeManagementTools*<br /><br />源<br /><br />*-计算机名称*<br /><br />-凭据<br /><br />-LogPath<br /><br />*-Vhd*<br /><br />*-ConfigurationFilePath*|  
  
> [!NOTE]  
> 不需要段时间，参数**-IncludeManagementTools**安装广告 DS 角色二进制文件时强烈建议  
  
ServerManager 模块为 Windows PowerShell 公开角色安装、状态和删除部分的 DISM 新模块。 此分层简化最任务，并减少需（但当滥用危险）功能强大的直接使用 DISM 模块。  
  
使用**获取命令**导出的别名和 cmdlet ServerManager 中的。  
  
```powershell  
Get-Command -module ServerManager  
```  
  
例如：  
  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSGetCommand.png)  
  
若要添加的 Active Directory 域服务角色，只需运行**安装 WindowsFeature**作为参数广告 DS 角色名称。 如服务器管理器中，所有所需的服务广告 DS 角色隐式自动安装。  
  
```powershell  
Install-WindowsFeature -name AD-Domain-Services  
```  
  
如果你还想要安装的广告 DS 管理工具，强烈建议-然后提供**-IncludeManagementTools**参数：  
  
```powershell  
Install-WindowsFeature -name AD-Domain-Services -IncludeManagementTools  
```  
  
例如：  
  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallWinFeature.png)  
  
若要在列表中的所有功能和与他们安装状态角色，使用**获取 WindowsFeature**无需参数。 指定**-ComputerName**参数远程服务器从安装状态。  
  
```powershell  
Get-WindowsFeature  
```  
  
因为**获取 WindowsFeature**没有筛选机制，必须使用**位置对象**与管道要查找特定功能。 管道是使用多个 cmdlet 来通过数据之间的频道，并 Where-Object cmdlet 充当筛选器。 内置**$_**变量充当通过与它可能包含的任何属性管道当前的对象。  
  
```powershell  
Get-WindowsFeature | where-object <options>  
```  
  
例如，若要查找的所有功能包含"活动目录"在其**显示名称**属性，使用：  
  
```powershell  
Get-WindowsFeature | where displayname -like "*active dir*"  
```  
  
更多示例下方所示：  
  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSGetWindowsFeature.png)  
  
有关更多的 Windows PowerShell 操作管道和 Where-Object 的详细信息，请参阅[管道，并且在 Windows PowerShell 管道](https://technet.microsoft.com/library/ee176927.aspx)。  
  
此外请注意的 Windows PowerShell 3.0 大大简化在此管道操作中所需的命令行参数。 Windows PowerShell 2.0 需要：  
  
```powershell  
Get-WindowsFeature | where {$_.displayname - like "*active dir*"}  
```  
  
通过使用 Windows PowerShell 管道的则可以创建较高的可读性结果。 例如：  
  
```powershell  
Install-WindowsFeature | Format-List  
Install-WindowsFeature | select-object | Format-List  
  
```  
  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDS.png)  
  
注意如何使用**选择对象**与 cmdlet **-expandproperty**参数返回有趣的数据：  
  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDSWithTools.png)  
  
> [!NOTE]  
> **选择对象 expandproperty**参数略有减慢整体安装性能。  
  
### <a name="BKMK_PS"></a>使用 Windows PowerShell 创建一个广告 DS 森林根域  
若要安装使用 ADDSDeployment 模块新 Active Directory 森林，使用以下 cmdlet:  
  
```powershell  
Install-addsforest  
```  
  
**安装 AddsForest** cmdlet 都仅有两个阶段（先决条件检查和安装）。 两个下图显示的最低要求参数了安装阶段**-域名**。  
  
|||  
|-|-|  
|ADDSDeployment Cmdlet|参数 (**加粗**参数是必需的。 *斜体*可以通过使用 Windows PowerShell 或广告 DS 配置向导指定参数。)|  
|Install-Addsforest|-确认<br /><br />*-CreateDNSDelegation*<br /><br />*-DatabasePath*<br /><br />*-DomainMode*<br /><br />***-域名***<br /><br />***-DomainNetBIOSName***<br /><br />*-DNSDelegationCredential*<br /><br />*-目录林*<br /><br />-强制<br /><br />*-InstallDNS*<br /><br />*-LogPath*<br /><br />-NoDnsOnNetwork<br /><br />-NoRebootOnCompletion<br /><br />*-SafeModeAdministratorPassword*<br /><br />-SkipAutoConfigureDNS<br /><br />-SkipPreChecks<br /><br />*-SYSVOLPath*<br /><br />*-Whatif*|  
  
> [!NOTE]  
> **-DomainNetBIOSName**参数是必需的如果你想要更改基于 DNS 域名称前缀自动生成的 15 个字符的名称，或者如果名称超出了 15 个字符。  
  
等效服务器管理器**部署配置**ADDSDeployment cmdlet 和参数是：  
  
```powershell  
Install-ADDSForest  
-DomainName <string>  
```  
  
等效服务器管理器的域控制器选项 ADDSDeployment cmdlet 参数是：  
  
```powershell  
-ForestMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>  
-DomainMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>  
-InstallDNS <{$false | $true}>  
-SafeModeAdministratorPassword <secure string>  
  
```  
  
**安装 ADDSForest**参数如果没有，请按照相同的默认值为服务器管理器。  
  
**SafeModeAdministratorPassword**特别是参数的操作：  
  
-   如果*未指定*作为参数，cmdlet 提示你输入并确认屏蔽的密码。 运行 cmdlet 交互时，这是首选的使用量。  
  
    例如，创建新的林名为 corp.contoso.com，力求尽可能系统提示您输入并确认屏蔽的密码：  
  
    ```powershell  
    Install-ADDSForest "DomainName corp.contoso.com  
    ```  
  
-   如果指定*值*的值必须安全字符串。 运行 cmdlet 交互时，这不是首选的使用量。  
  
例如，你可以手动提示输入密码使用**读取主机**cmdlet 安全字符串用户提示：  
  
```powershell  
-safemodeadministratorpassword (read-host -prompt "Password:" -assecurestring)  
```  
  
> [!WARNING]  
> 上一个选项没有确认密码，如使用格外小心：的密码不可见。  
  
虽然这是强烈建议不要，你还可以为转换清除文本变量，提供安全的字符串。  
  
```powershell  
-safemodeadministratorpassword (convertto-securestring "Password1" -asplaintext -force)  
```  
  
最后，无法将模糊的密码存储在某个文件，然后重用它更高版本，而清晰的文本密码会显示。 例如：  
  
```powershell  
$file = "c:\pw.txt"  
$pw = read-host -prompt "Password:" -assecurestring  
$pw | ConvertFrom-SecureString | Set-Content $file  
  
-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)  
  
```  
  
> [!WARNING]  
> 不建议提供或存储清除或模糊短的密码。 脚本中运行此命令或查找通过您的任何人都知道 DSRM 域控制器的密码。 有权访问文件的任何人都可以反转该模糊的密码。 与该知识，他们可以登录到 DC DSRM 以启动并最终模拟域控制器本身提升最大程度 Active Directory 森林中的权限。 步骤，使用另一组**System.Security.Cryptography**文本的文件进行加密数据是建议，但利用范围。 最佳做法是完全避免密码存储。  
  
ADDSDeployment cmdlet 提供其他选项，可跳过自动配置 DNS 客户端设置、转发器，以及根提示。 使用服务器管理器时，无法跳过此配置选项。 此参数很重要，仅当你安装 DNS 服务器前配置域控制器角色：  
  
```powershell  
-SkipAutoConfigureDNS  
```  
  
**DomainNetBIOSName**操作也是特殊：  
  
-   如果**DomainNetBIOSName** NetBIOS 域名称和中的单标签前缀域名未指定参数**域名**参数 15 字符或更少，则推广继续自动生成的名称。  
  
-   如果**DomainNetBIOSName** NetBIOS 域名称和中的单标签前缀域名未指定参数**域名**参数为 16 个字符或的详细信息，然后升级失败。  
  
-   如果**DomainNetBIOSName**参数指定 NetBIOS 域名为 15 字符或更少，然后升级继续使用该指定的名称。  
  
-   如果**DomainNetBIOSName**参数指定 16 个字符的 NetBIOS 域名称或的详细信息，然后升级失败。  
  
等效服务器管理器的其他选项 ADDSDeployment cmdlet 参数是：  
  
```powershell  
-domainnetbiosname <string>  
```  
  
等效服务器管理器**路径**ADDSDeployment cmdlet 参数均是：  
  
```powershell  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
  
```  
  
使用可选**Whatif**具有参数**安装 ADDSForest** cmdlet 查看配置的信息。 这使你查看 cmdlet 参数明确和隐式值。  
  
例如：  
  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSPaths.png)  
  
你无法跳过**先决条件检查**时使用服务器管理器中，但你可以跳过此过程时使用使用下面的参数广告 DS 部署 cmdlet:  
  
```powershell  
-skipprechecks  
```  
  
> [!WARNING]  
> Microsoft 不鼓励跳过先决条件检查，因为它可能会导致部分域控制器升级或损坏广告 DS 森林。  
  
注意如何操作，就像服务器管理器**安装 ADDSForest**提醒你，升级将自动重新启动的服务器。  
  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSReboot.png)  
  
![安装新森林](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallProgress.png)  
  
若要自动接受重新引导提示，请使用**-强制**或**-确认：$false**与任何 ADDSDeployment Windows PowerShell cmdlet 参数。 若要防止服务器推广末尾自动重新启动，请使用**-norebootoncompletion**参数。  
  
> [!WARNING]  
> 不建议重重新启动。 域控制器必须重新启动才能正常工作。  
  
## <a name="see-also"></a>请参阅  
[Active Directory 域服务（TechNet 门户）](https://technet.microsoft.com/library/cc770946(WS.10).aspx)  
[Windows Server 2008 r2 的 Active Directory 域服务](https://technet.microsoft.com/library/dd378801(WS.10).aspx)  
[适用于 Windows Server 2008 Active Directory 域服务](https://technet.microsoft.com/library/dd378891(WS.10).aspx)  
[Windows Server 技术参考 (Windows Server 2003)](https://technet.microsoft.com/library/cc739127(WS.10).aspx)  
[Active Directory 管理中心：入门 (Windows Server 2008 R2)](https://technet.microsoft.com/library/dd560651(WS.10).aspx)  
[Active Directory(Windows Server 2008 R2) 的 Windows PowerShell 与管理](https://technet.microsoft.com/library/dd378937(WS.10).aspx)  
[询问目录服务团队（官方 Microsoft 商业技术支持博客）](http://blogs.technet.com/b/askds)  
  

