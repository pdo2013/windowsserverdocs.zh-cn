---
ms.assetid: b146f47e-3081-4c8e-bf68-d0f993564db2
title: 虚拟化域控制器部署和配置
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: be2c919e4379cf615fe25d68446855229ace87dd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390704"
---
# <a name="virtualized-domain-controller-deployment-and-configuration"></a>虚拟化域控制器部署和配置

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主题讲解：  
  
-   [安装注意事项](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_InstallConsiderations)  
  
    本部分包括平台要求和其他重要限制。  
  
-   [虚拟化域控制器克隆](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_VDCCloning)  
  
    本部分详细说明整个虚拟化域控制器克隆过程。  
  
-   [虚拟化域控制器安全还原](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_VDCSafeRestore)  
  
    本部分详细说明在虚拟化域控制器安全还原期间所进行的验证。  
  
## <a name="BKMK_InstallConsiderations"></a>安装注意事项  
虚拟化域控制器不存在任何特殊的角色或功能安装；所有域控制器都将自动包含克隆和安全还原功能。 无法删除或禁用这些功能。  
  
使用 Windows Server 2012 域控制器要求 Windows Server 2012 AD DS 架构版本 56 或更高版本，以及与 Windows Server 2003 本机或更高版本相等的林功能级别。  
  
可写和只读域控制器都支持虚拟化 DC 的所有方面，全局编录和 FSMO 角色也是如此。  
  
> [!IMPORTANT]  
> 克隆开始时，PDC 模拟器 FSMO 角色所有者必须处于联机状态。  
  
### <a name="BKMK_PlatformReqs"></a>平台要求  
虚拟化域控制器克隆要求：  
  
-   Windows Server 2012 DC 上托管的 PDC 模拟器 FSMO 角色  
  
-   克隆操作期间 PDC 模拟器可用  
  
克隆和安全还原都要求：  
  
-   Windows Server 2012 虚拟化来宾  
  
-   虚拟化主机平台支持 VM 生成 ID (VMGID)  
  
查看下表中的虚拟化产品以及它们是否支持虚拟化域控制器和 VM 生成 ID。  
  
|||  
|-|-|  
|**虚拟化产品**|**支持虚拟化域控制器和 VMGID**|  
|**带有 Hyper-v 功能的 Microsoft Windows Server 2012 服务器**|是|  
|**Microsoft Windows Server 2012 Hyper-v 服务器**|是|  
|**具有 Hyper-v 客户端功能的 Microsoft Windows 8**|是|  
|**Windows Server 2008 R2 和 Windows Server 2008**|否|  
|**非 Microsoft 虚拟化解决方案**|联系供应商|  
  
即使 Microsoft 支持 Windows 7 Virtual PC、Virtual PC 2007、Virtual PC 2004 和 Virtual Server 2005，它们也无法运行 64 位来宾，无法支持 VM 生成 ID。  
  
要获取有关第三方虚拟化产品及其对虚拟化域控制器的支持情况的帮助，请直接联系该供应商。  
  
有关详细信息，请查看 [在非 Microsoft 硬件虚拟化软件中运行的 Microsoft 软件](https://support.microsoft.com/kb/897615)的支持策略。  
  
### <a name="critical-caveats"></a>重要事项  
虚拟化域控制器*不*支持下列对象的安全还原：  
  
-   手动复制以覆盖现有 VHD 文件的 VHD 和 VHDX 文件  
  
-   使用文件备份或全磁盘备份软件还原的 VHD 和 VHDX 文件  
  
> [!NOTE]  
> VHDX 文件是 Windows Server 2012 Hyper-V 中的新增文件。  
  
这两个操作都不包含在 VM 生成 ID 语义下，因此不会更改 VM 生成 ID。 使用这些方法还原域控制器可能会导致 USN 回滚，也可能会隔离域控制器或引入延迟对象并导致需要在整个林中进行清理操作。  
  
> [!WARNING]  
> 虚拟化域控制器安全还原不是系统状态备份和 AD DS 回收站的替代功能。  
>   
> 还原快照之后，在快照后源自该域控制器的之前未复制的更改增量将永远丢失。 安全还原将实现自动化的非权威还原， *仅*为了防止意外的域控制器隔离。  
  
有关 USN 气泡和延迟对象的详细信息，请参阅 [Troubleshooting Active Directory 操作失败并出现错误8606："提供的属性不足以创建对象" ](https://support.microsoft.com/kb/2028495)。  
  
## <a name="BKMK_VDCCloning"></a>虚拟化域控制器克隆  
无论使用图形工具或 Windows PowerShell，都存在大量用于克隆虚拟化域控制器的阶段和步骤。 在较高级别，这三个阶段为：  
  
**准备环境**  
  
-   第 1 步：验证虚拟机监控程序支持 VM 生成 ID，并因此可以进行克隆  
  
-   步骤 2：验证 PDC 仿真器角色是否由运行 Windows Server 2012 的域控制器托管，以及在克隆期间克隆的域控制器是否处于联机状态。  
  
**准备源域控制器**  
  
-   步骤 3:授予源域控制器克隆权限  
  
-   步骤 4：删除不兼容的服务或程序，或将其添加到 Customdccloneallowlist.xml 文件。  
  
-   步骤 5：创建 DCCloneConfig.xml  
  
-   步骤 6：使源域控制器脱机  
  
**创建克隆的域控制器**  
  
-   步骤 7：复制或导出源 VM，并添加 XML（若尚未复制）  
  
-   步骤 8：从副本创建新的虚拟机  
  
-   步骤 9：启动新的虚拟机，开始克隆  
  
使用图形工具（如 Hyper-V 管理控制台）或命令行工具（如 Windows PowerShell）执行的操作在过程上没有差别，因此仅介绍一次这些用于两个界面的步骤。 本主题提供了 Windows PowerShell 示例，以供你了解克隆过程的端到端自动化；对于任何步骤，它们都非必需。 Windows Server 2012 中不包含用于虚拟化域控制器的图形管理工具。  
  
在该过程存在几个点，你可以在那时选择如何创建克隆的计算机以及如何添加 xml 文件；下面详细注明了这些步骤。 此过程不可以其他方式更改。  
  
下图说明了虚拟化域控制器克隆的过程，其中域已经存在。  
  
![虚拟化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_CloningProcessFlow.png)  
  
### <a name="step-1---validate-the-hypervisor"></a>步骤 1 – 验证虚拟机监控程序  
通过查看供应商文档，确保源域控制器在支持的虚拟机监控程序上运行。 虚拟化域控制器与虚拟机监控程序无关，且不需要 Hyper-v。  
  
如果虚拟机监控程序是 Microsoft Hyper-V 的，请确保它在 Windows Server 2012 上运行。 你可以使用设备管理对此进行验证  
  
打开 **Devmgmt.msc** 并检查**系统设备**中的已安装 Microsoft Hyper-V 设备和驱动程序。 虚拟化域控制器需要的特定系统设备是 **Microsoft Hyper-V 生成计数器** （驱动程序：vmgencounter.sys）。  
  
![虚拟化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVVMGenIDCounter.png)  
  
### <a name="step-2---verify-the-pdce-fsmo-role"></a>步骤 2 – 验证 PDCE FSMO 角色  
在尝试克隆 DC 之前，你必须验证托管主域控制器模拟器 FSMO 的域控制器运行 Windows Server 2012。 PDC 模拟器 (PDCE） 是必需的，原因有多种：  
  
1.  PDCE 将创建特殊的 **可克隆的域控制器** 组，并在域的根上设置其权限以允许域控制器克隆其自身。  
  
2.  克隆域控制器将使用 DRSUAPI RPC 协议直接联系 PDCE，以便为克隆 DC 创建计算机对象。  
  
    > [!NOTE]  
    > Windows Server 2012 将扩展现有的目录复制服务 (DRS) 远程协议 (UUID **E3514235-4B06-11D1-AB04-00C04FC2DCD2**) 以囊括新的 RPC 方法 **IDL_DRSAddCloneDC** (Opnum **28**)。 **IDL_DRSAddCloneDC** 方法通过复制现有域控制器对象中的属性来创建新的域控制器对象。  
    >   
    > 域控制器的状态由为每个域控制器保留的计算机、服务器、NTDS 设置、FRS、DFSR 和连接对象组成。 在复制对象时，此 RPC 方法会将对原始域控制器的所有引用替换为新域控制器的相应对象。 调用方必须具有域命名上下文中的控制访问权 DS-Clone-Domain-Controller。  
    >   
    > 使用此新方法始终需要从调用方直接访问 PDC 模拟器域控制器。  
    >   
    > 因为此 RPC 方法是新方法，所以你的网络分析软件需要更新的分析程序，以囊括现有 UUID E3514235-4B06-11D1-AB04-00C04FC2DCD2 中关于新 Opnum 28 的字段。 否则，你无法分析该流量。  
    >   
    > 有关详细信息，请参阅 [4.1.29 IDL_DRSAddCloneDC (Opnum 28)](https://msdn.microsoft.com/library/hh554213(v=prot.13).aspx)。  
  
***也就是说，当使用非完全路由的网络时，虚拟化域控制器克隆需要有权访问 PDCE 的网络段***。 只要你非常小心地更新 AD DS 逻辑站点信息，在克隆后可以将克隆的域控制器移到另一个网络（类似于物理域控制器）。  
  
> [!IMPORTANT]  
> 克隆仅包含单个域控制器的域时，必须确保源 DC 在开始克隆副本之前已重新联机。 生产域应始终包含至少两个域控制器。  
  
#### <a name="active-directory-users-and-computers-method"></a>Active Directory 用户和计算机方法  
  
1.  使用 Dsa.msc 管理单元，右键单击域，然后单击 **操作主机**。 注意在 PDC 选项卡上命名的域控制器，然后关闭对话框。  
  
2.  右键单击该 DC 的计算机对象，再单击**属性**，然后验证操作系统信息。  
  
#### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
你可以结合以下的 Active Directory Windows PowerShell 模块 cmdlet 以返回 PDC 模拟器版本：  
  
```  
Get-adddomaincontroller  
Get-adcomputer  
```  
  
如果未提供域，则这些在计算机中运行的 cmdlet 将假定该计算机的域。  
  
下面的命令会返回 PDCE 和操作系统的信息：  
  
```  
get-adcomputer(Get-ADDomainController -Discover -Service "PrimaryDC").name -property * | format-list dnshostname,operatingsystem,operatingsystemversion  
```  
  
以下示例演示了如何在 Windows PowerShell 管道之前指定域名并筛选返回的属性：  
  
![虚拟化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PDCOSInfo.png)  
  
### <a name="step-3---authorize-a-source-dc"></a>步骤 3-授权源 DC  
源域控制器必须具有对域 NC 头的控制访问权限 (CAR) **允许 DC 创建其自身的克隆**。 默认情况下，已知组 **可克隆的域控制器** 具有此权限，并且不包含任何成员。 该 FSMO 角色传输到 Windows Server 2012 域控制器后，PDCE 将创建此组。  
  
#### <a name="active-directory-administrative-center-method"></a>Active Directory 管理中心方法  
  
1.  启动 Dsac.exe 并导航到源 DC，然后打开其详细信息页面。  
  
2.  在 **隶属于** 部分，为该域添加 **可克隆的域控制器** 组。  
  
#### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
你可以结合以下 Active Directory Windows PowerShell 模块 cmdlet **get-adcomputer** 和 **add-adgroupmember**，将域控制器添加到“可克隆的域控制器”组：  
  
```  
Get-adcomputer <dc name> | %{add-adgroupmember "cloneable domain controllers" $_.samaccountname}  
```  
  
例如，这种方法将服务器 DC1 添加到组中，而无需指定组成员的可分辨名称：  
  
![虚拟化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_AddDcToGroup.png)  
  
#### <a name="rebuilding-default-permissions"></a>重建默认权限  
如果从域头中删除此权限，则克隆将失败。 你可以使用 Active Directory 管理中心或 Windows PowerShell 重新创建权限。  
  
##### <a name="active-directory-administrative-center-method"></a>Active Directory 管理中心方法  
  
1.  打开 **Active Directory 管理中心**，右键单击域头，单击 **属性**，单击 **扩展** 选项卡，单击 **安全**，然后单击 **高级**。 单击**仅此对象**。  
  
2.  单击**添加**，在**输入要选择的对象名称**下，键入组名**可克隆的域控制器。**  
  
3.  在“权限”下，单击 **允许 DC 创建其自身的克隆**，然后单击 **确定**。  
  
> [!NOTE]  
> 你还可以删除默认权限，并添加单个域控制器。 但是，执行此操作可能会导致持续不断的维护问题，并且新的管理员不会意识到此自定义。 更改默认设置不会增加安全性，且不建议这样做。  
  
##### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
在提升为管理员身份的 Windows PowerShell 控制台提示中使用以下命令。 这些命令将检测域名，并将默认权限重新添加进来：  
  
```  
import-module activedirectory  
cd ad:  
$domainNC = get-addomain  
$dcgroup = get-adgroup "Cloneable Domain Controllers"  
$sid1 = (get-adgroup $dcgroup).sid  
$acl = get-acl $domainNC  
$objectguid = new-object Guid 3e0f7e18-2c7a-4c10-ba82-4d926db99a3e  
$ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid1,"ExtendedRight","Allow",$objectguid  
$acl.AddAccessRule($ace1)  
set-acl -aclobject $acl $domainNC  
cd c:  
```  
  
此外，在 Windows PowerShell 控制台中运行该示例 [FixVDCPermissions.ps1](../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_FixPDCPerms) ，其中控制台将在受影响域中的域控制器上以提升的管理员身份启动。 它将自动设置权限。 该示例位于此模块的附录中。  
  
### <a name="step-4---remove-incompatible-applications-or-services-if-not-using-customdccloneallowlistxml"></a>步骤 4 – 删除不兼容的应用程序或服务（如果不使用 CustomDCCloneAllowList.xml）  
在克隆之前，必须删除任何之前由 Get-ADDCCloningExcludedApplicationList *返回（且未添加到 CustomDCCloneAllowList.xml* ）的程序或服务。 卸载应用程序或服务是建议的方法。  
  
> [!WARNING]  
> 任何未卸载或添加到 CustomDCCloneAllowList.xml 的不兼容程序或服务都会阻止克隆。  
  
使用 Get-AdComputerServiceAccount cmdlet 在域中查找任何独立的托管服务帐户 (MSA)，并检查此计算机是否正在使用其中任何帐户。 如果已安装任一 MSA，则使用 Uninstall-ADServiceAccount cmdlet 来删除本地已安装的服务帐户。 完成第 6 步中使源域控制器脱机的操作后，在服务器重新联机时，你可以使用 Install-ADServiceAccount 重新添加 MSA。 有关详细信息，请参阅 [Uninstall-adserviceaccount](https://technet.microsoft.com/library/hh852310)。  
  
> [!IMPORTANT]  
> 在 Windows Server 2012 中，已将独立 MSA（在 Windows Server 2008 R2 中首次发布）替换为组 MSA。 组 MSA 支持克隆。  
  
### <a name="step-5---create-dccloneconfigxml"></a>步骤 5 – 创建 DCCloneConfig.xml  
克隆域控制器需要 DcCloneConfig.xml 文件。 其内容允许你指定唯一的详细信息，如新的计算机名和 IP 地址。  
  
除非你在源域控制器上安装应用程序或可能不兼容的 Windows 服务，否则 CustomDCCloneAllowList.xml 文件是可选的。 这些文件需要精确地命名、设置格式和放置；否则，克隆将失败。  
  
为此，你应当始终使用 Windows PowerShell cmdlet 创建 XML 文件并将其放在正确的位置。  
  
#### <a name="generating-with-new-addccloneconfigfile"></a>使用 New-ADDCCloneConfigFile 生成  
Active Directory Windows PowerShell 模块包含 Windows Server 2012 中的新 cmdlet：  
  
```  
New-ADDCCloneConfigFile  
```  
  
你将在打算克隆的建议的源域控制器上运行该 cmdlet。 该 cmdlet 支持多个参数，而且在使用时，将始终测试计算机及其运行的环境，除非你指定 -offline 参数。  
  
||||  
|-|-|-|  
|**Active**<br /><br />**Cmdlet**|**参数**|**做出**|  
|**新-New-addccloneconfigfile**|*<no argument specified>*|在 DSA 工作目录中创建空白 DcCloneConfig.xml 文件（默认位置：%systemroot%\ntds）|  
||-CloneComputerName|指定克隆 DC 计算机名。 字符串数据类型。|  
||-Path|指定文件夹以创建 DcCloneConfig.xml。 如果未指定，则将写入 DSA 工作目录（默认位置：%systemroot%\ntds）。 字符串数据类型。|  
||-SiteName|指定在克隆的计算机帐户创建过程中要加入的 AD 逻辑站点名称。 字符串数据类型。|  
||-IPv4Address|指定克隆的计算机的静态 IPv4 地址。 字符串数据类型。|  
||-IPv4SubnetMask|指定克隆的计算机的静态 IPv4 子网掩码。 字符串数据类型。|  
||-IPv4DefaultGateway|指定克隆的计算机的静态 IPv4 默认网关地址。 字符串数据类型。|  
||-IPv4DNSResolver|在以逗号分隔的列表中，指定克隆的计算机的静态 IPv4 DNS 条目。 数组数据类型。 最多可以提供四个条目。|  
||-PreferredWINSServer|指定主 WINS 服务器的静态 IPv4 地址。 字符串数据类型。|  
||-AlternateWINSServer|指定辅助 WINS 服务器的静态 IPv4 地址。 字符串数据类型。|  
||-IPv6DNSResolver|在以逗号分隔的列表中，指定克隆的计算机的静态 IPv6 DNS 条目。 无法设置虚拟化域控制器克隆中的 Ipv6 静态信息。 数组数据类型。|  
||-Offline|不会执行验证测试并将覆盖任何现有的 dccloneconfig.xml。 无参数。|  
||*-Static*|如果指定静态 IP 参数 IPv4SubnetMask、IPv4SubnetMask 或 IPv4DefaultGateway，则需要该参数。 无参数。|  
  
在联机模式下运行时执行的测试：  
  
-   PDC 模拟器为 Windows Server 2012 或更高版本  
  
-   源域控制器为“可克隆的域控制器”组的成员  
  
-   源域控制器不包含任何已排除的应用程序或服务  
  
-   源域控制器尚未包含指定路径上的 DcCloneConfig.xml  
  
![虚拟化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSNewDCCloneConfig.png)  
  
### <a name="step-6---take-the-source-domain-controller-offline"></a>步骤 6 – 使源域控制器脱机  
不可以复制运行中的源 DC；必须适当地关闭它。 请勿克隆因不适当断电而停止运行的域控制器。  
  
#### <a name="graphical-method"></a>图形方法  
使用运行中的 DC 中的关闭按钮或 Hyper-V 管理器关闭按钮。  
  
![虚拟化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_Shutdown.png)  
  
![虚拟化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVShutdown.png)  
  
#### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
可以使用以下 cmdlet 之一关闭虚拟机：  
  
```  
Stop-computer  
Stop-vm  
```  
  
Stop-computer 是一个支持在不考虑虚拟化的情况下关闭计算机的 cmdlet，它类似于传统 Shutdown.exe 实用程序。 Stop-vm 是 Windows Server 2012 Hyper-V Windows PowerShell 模块中的一个新 cmdlet，等效于 Hyper-V 管理器中的电源选项。 后者在实验室环境中非常有用，其中域控制器经常在专用的虚拟化网络上运行。  
  
![虚拟化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_StopComputer2.png)  
  
![虚拟化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_StopVM.png)  
  
### <a name="step-7---copy-disks"></a>步骤 7 – 复制磁盘  
在复制阶段，要求作出一个管理方面的选择：  
  
-   不使用 Hyper-V，手动复制磁盘  
  
-   使用 Hyper-V 导出 VM  
  
-   使用 Hyper-V 导出合并的磁盘  
  
必须复制一台虚拟机的所有磁盘，而不仅仅复制系统驱动器。 如果源域控制器使用差异磁盘，并且你打算将克隆的域控制器移到另一台 Hyper-V 主机上，则你必须导出。  
  
如果源域控制器仅具有*一个*驱动器，则建议手动复制磁盘。 建议为具有 *多个* 驱动器或其他复杂的虚拟化硬件自定义（如多个 NIC）的 VM 使用导出/导入。  
  
如果手动复制文件，请在复制之前删除任何快照。 如果导出 VM，请在导出之前删除快照，或在导入后从新的 VM 删除它们。  
  
> [!WARNING]  
> 快照是可以将域控制器返回到以前状态的差异磁盘。 如果你打算克隆域控制器，然后还原其预克隆的快照，则最后你的林中将存在重复的域控制器。 在新克隆的域控制器上，之前的快照没有价值。  
  
#### <a name="manually-copying-disks"></a>手动复制磁盘  
  
##### <a name="hyper-v-manager-method"></a>Hyper-V 管理器方法  
使用 Hyper-V 管理器管理单元来确定哪些磁盘与源域控制器相关联。 使用“检查”选项来验证域控制器是否使用差异磁盘（还要求你复制父磁盘）  
  
![虚拟化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVInspect.png)  
  
若要删除快照，请选择一个 VM，并删除快照子树。  
  
![虚拟化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVDeleteSnapshot.gif)  
  
然后，你可以使用 Windows 资源管理器、Xcopy.exe 或 Robocopy.exe 手动复制 VHD 或 VHDX 文件。 无需任何特殊步骤。 即使移动到另一个文件夹，更改文件名称仍是最佳实践。  
  
> [!NOTE]  
> 如果在 LAN（1 Gbit 或更大）上的主计算机之间进行复制，则使用 **Xcopy.exe /J** 选项复制 VHD/VHDX 文件将比任何其他工具快得多，但也会占用更大的带宽。  
  
##### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
若要确定使用 Windows PowerShell 的磁盘，请使用 Hyper-V 模块：  
  
```  
Get-vmidecontroller  
Get-vmscsicontroller  
Get-vmfibrechannelhba  
Get-vmharddiskdrive  
```  
  
例如，你可以使用下面的示例，从名为 **DC2** 的 VM 中返回所有 IDE 硬盘驱动器：  
  
![虚拟化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_ReturnIDE.png)  
  
如果磁盘路径指向 AVHD 或 AVHDX 文件，则它是快照。 若要删除与磁盘相关联的快照并合并到实际的 VHD 或 VHDX 中，请使用 cmdlet：  
  
```  
Get-VMSnapshot  
Remove-VMSnapshot  
```  
  
例如，若要从名为 DC2-SOURCECLONE 的 VM 中删除所有快照：  
  
![虚拟化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_DelSnapshots.png)  
  
若要使用 Windows PowerShell 复制这些文件，请使用以下 cmdlet：  
  
```  
Copy-Item  
```  
  
与的管道中的 VM cmdlet 相结合以协助自动化。 管道是在多个 cmdlet 之间用于传递数据的一个通道。 例如，要将名为 DC2-SOURCECLONE 的脱机源域控制器的驱动器复制到名为 c:\temp\copy.vhd 的新磁盘上，而无需知道其系统驱动器的准确路径，请执行以下操作：  
  
```  
Get-VMIdeController dc2-sourceclone | Get-VMHardDiskDrive | select-Object {copy-item -path $_.path -destination c:\temp\copy.vhd}  
```  
  
![虚拟化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSCopyDrive.png)  
  
> [!IMPORTANT]  
> 你无法通过克隆使用 passthru 磁盘，因为它们并不使用虚拟磁盘文件，而是使用实际硬盘。  
  
> [!NOTE]  
> 有关管道的更多 Windows PowerShell 操作的详细信息，请参阅 [Windows PowerShell 中的管道系统和管道](https://technet.microsoft.com/library/ee176927.aspx)。  
  
#### <a name="exporting-the-vm"></a>导出 VM  
作为复制磁盘的替代方法，可以将整个 Hyper-V VM 导出为副本。 导出将自动创建一个为 VM 命名的包含所有磁盘和配置信息的文件夹。  
  
![虚拟化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVExport.png)  
  
##### <a name="hyper-v-manager-method"></a>Hyper-V 管理器方法  
若要使用 Hyper-V 管理器导出 VM：  
  
1.  右键单击源域控制器，然后单击 **导出**。  
  
2.  选择一个现有文件夹作为导出容器。  
  
3.  等待“状态”列停止显示 **正在导出**。  
  
##### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
若要使用 Hyper-V Windows PowerShell 模块导出 VM，请使用 cmdlet：  
  
```  
Export-vm  
```  
  
例如，将名为 DC2-SOURCECLONE 的 VM 导出到名为 C:\VM 的文件夹：  
  
![虚拟化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSExport.png)  
  
> [!NOTE]  
> Windows Server 2012 Hyper-V 支持新的导出和导入功能，这些功能在此培训范围以外。 请查看 TechNet 获取详细信息。  
  
#### <a name="exporting-merged-disks-using-hyper-v"></a>使用 Hyper-V 导出合并的磁盘  
最后一个选项是使用 Hyper-V 中的磁盘合并和转换选项。 这些选项允许你将现有磁盘结构的副本（即使在包括快照 AVHD/AVHDX 文件时）制作为单个新磁盘。 与手动磁盘复制方案一样，此方案主要适用于仅使用单个驱动器的更简单的虚拟机，例如 C： \\。 其唯一的优点是，与手动复制不同，它不需要你首先删除快照。 此操作一定比仅删除快照和复制磁盘慢。  
  
##### <a name="hyper-v-manager-method"></a>Hyper-V 管理器方法  
使用 Hyper-V 管理器创建合并的磁盘：  
  
1.  单击 "**编辑磁盘**"。  
  
2.  浏览最低的子磁盘。 例如，如果使用差异磁盘，则子磁盘是最低的子项。 如果虚拟机含有单个快照（或多个快照），则当前选定的快照是最低的子磁盘。  
  
3.  选择 “合并”选项，以在整个父子结构外创建单一磁盘。  
  
4.  选择新的虚拟硬盘并提供路径。 此操作将现有 VHD/VHDX 文件协调为单个新便携式单元，该单元不存在还原以前快照的风险。  
  
##### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
若要使用 Hyper-V Windows PowerShell 模块从一组复杂的父磁盘创建合并的磁盘，请使用 cmdlet：  
  
```  
Convert-vm  
```  
  
例如，要将 VM 的磁盘快照（这次不包括任何差异磁盘）和父磁盘的整个链导出到名为 DC4-CLONED.VHDX 的新的单一磁盘：  
  
![虚拟化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSConvertVhd.png)  
  
#### <a name="BKMK_Offline"></a>将 XML 添加到脱机系统磁盘  
如果你将 Dccloneconfig.xml 复制到运行中的源 DC，则你现在必须将更新的 dccloneconfig.xml 文件复制到脱机复制/导出的系统盘。 具体取决于之前使用 Get-ADDCCloningExcludedApplicationList 检测到的已安装应用程序，你可能还需要将 CustomDCCloneAllowList.xml 文件复制到磁盘。  
  
以下位置可能包含 DcCloneConfig.xml 文件：  
  
1.  DSA 工作目录  
  
2.  %windir%\NTDS  
  
3.  按照驱动器号顺序排列的可移动读/写媒体，位于驱动器根目录下  
  
这些路径是不可配置的。 在克隆开始后，克隆将按这个特定的顺序检查这些位置，并使用找到的第一个 DcCloneConfig.xml 文件，而不管其他文件夹的内容。  
  
以下位置可能包含 CustomDCCloneAllowList.xml 文件：  
  
1.  HKey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters  
  
    AllowListFolder (*REG_SZ*)  
  
2.  DSA 工作目录  
  
3.  %windir%\NTDS  
  
4.  按照驱动器号顺序排列的可移动读/写媒体，位于驱动器根目录下  
  
你可以使用 **-offline** 参数（也称为脱机模式）运行 New-ADDCCloneConfigFile，以创建 DcCloneConfig.xml 文件并将其放在正确的位置上。 下面的示例将演示如何在脱机模式下运行 New-ADDCCloneConfigFile。  
  
若要在脱机模式下创建名为 CloneDC1 的克隆域控制器，请在名为 "REDMOND" 且带有静态 IPv4 地址的站点中键入：  
  
```  
New-ADDCCloneConfigFile -Offline -CloneComputerName CloneDC1 -SiteName REDMOND -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -Static -Path F:\Windows\NTDS  
```  
  
要在脱机模式下，使用静态 IPv4 和 IPv6 的静态设置创建名为 Clone2 的克隆域控制器，请键入：  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone2" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -Path F:\Windows\NTDS  
```  
  
要在脱机模式下，使用静态 IPv4 和 动态 IPv6 设置创建克隆域控制器，同时指定针对 DNS 解析程序设置的 DNS 服务器，请键入：  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.10" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -IPv4DNSResolver @( "10.0.0.1","10.0.0.2" ) -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS   
```  
  
要在脱机模式下用动态 IPv4 和静态 IPv6 设置创建名为 Clone1克隆域控制器，请键入：  
  
```  
New-ADDCCloneConfigFile -Offline -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone1" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -SiteName "REDMOND" -Path F:\Windows\NTDS  
```  
  
要在脱机模式下用动态 IPv4 和动态 IPv6 设置创建克隆域控制器，请键入：  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4DNSResolver "10.0.0.1" -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS  
  
```  
  
##### <a name="windows-explorer-method"></a>Windows 资源管理器方法  
现在，Windows Server 2012 为装载 VHD 和 VHDX 文件提供了一个图形选项。 此方法要求在 Windows Server 2012 上安装桌面体验功能。  
  
1.  单击包含源 DC 的系统驱动器或 DSA 工作目录位置文件夹的新复制的 VHD/VHDX 文件，然后在**光盘映像工具**菜单中单击**装载**。  
  
2.  在现装载的驱动器中，将 XML 文件复制到有效位置。 系统可能会提示你需要关于该文件夹的权限。  
  
3.  单击已装载的驱动器，然后在**磁盘工具**菜单中单击**弹出**。  
  
![虚拟化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVClickMountedDrive.png)  
  
![虚拟化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVDetailsMountedDrive.gif)  
  
![虚拟化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVEjectMountedDrive.gif)  
  
##### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
另外，你可以使用 Windows PowerShell cmdlet 装载脱机磁盘并复制 XML 文件：  
  
```  
mount-vhd  
get-disk  
get-partition  
get-volume  
Add-PartitionAccessPath  
Copy-Item  
```  
  
此方法允许你完全控制该过程。 例如，可以使用特定的驱动器号来装载驱动器，并且可以复制文件和卸载驱动器。  
  
```  
mount-vhd <disk path> -passthru -nodriveletter | get-disk | get -partition | get-volume | get-partition | where {$_.partition number -eq 2} | Add-PartitionAccessPath -accesspath <drive letter>  
  
copy-item <xml file path><destination path>\dccloneconfig.xml  
  
dismount-vhd <disk path>  
```  
  
例如：  
  
![虚拟化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSMountVHD.png)  
  
此外，你可以使用新的 **Mount-DiskImage** cmdlet 来装载 VHD（或 ISO）文件。  
  
### <a name="step-8---create-the-new-virtual-machine"></a>步骤 8 – 创建新的虚拟机  
在开始克隆过程之前的最后一个配置步骤是：创建一个新 VM，它使用复制的源域控制器中的磁盘。 根据在复制磁盘阶段所做的选择，你有两个选项：  
  
1.  将新的 VM 与复制的磁盘相关联  
  
2.  导入已导出的虚拟机  
  
#### <a name="associating-a-new-vm-with-copied-disks"></a>将新 VM 与复制的磁盘相关联  
如果你已手动复制系统盘，则你必须使用复制的磁盘创建新虚拟机。 创建新的 VM 后，虚拟机监控程序将自动设置 VM 生成 ID；在 VM 或 Hyper-V 主机中不需要进行任何配置更改。  
  
![虚拟化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVConnectVHD.gif)  
  
##### <a name="hyper-v-manager-method"></a>Hyper-V 管理器方法  
  
1.  创建新的虚拟机  
  
2.  指定 VM 名称、内存和网络。  
  
3.  在连接虚拟硬盘页上，指定复制的系统盘。  
  
4.  完成向导以创建 VM。  
  
如果存在多个磁盘、网络适配器或其他自定义，则请在启动域控制器之前配置它们。 建议为复杂的 VM 使用用于复制磁盘的“导出/导入”方法。  
  
##### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
使用以下 cmdlet，你可以在 Windows Server 2012 中使用 Hyper-V Windows PowerShell 模块自动化 VM 创建：  
  
```  
New-VM  
```  
  
例如，此处创建了 DC4-CLONEDFROMDC2 VM（使用 1 GB 的 RAM），它从 c:\vm\dc4-systemdrive-clonedfromdc2.vhd 文件启动并使用 10.0 虚拟网络创建：  
  
![虚拟化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSNewVM.png)  
  
#### <a name="import-vm"></a>导入 VM  
如果你之前已导出 VM，则你现在必须将它作为副本重新导入。 此方法使用导出的 XML 重新创建使用所有以前的设置、驱动器、网络和内存设置的计算机。  
  
如果你打算从同一导出的 VM 创建其他副本，则请根据需要创建足够多的已导出 VM 的副本。 然后对每个副本使用“导入”。  
  
> [!IMPORTANT]  
> 请务必使用 **副本** 选项，因为导出保留了源中的所有信息；如果在同一台 Hyper-V 主机服务器上使用 **移动** 或 **就地** 导入服务器，则此操作将导致信息冲突。  
  
##### <a name="hyper-v-manager-method"></a>Hyper-V 管理器方法  
若要使用 Hyper-V 管理器管理单元进行导入：  
  
1.  单击**导入虚拟机**  
  
2.  在**定位文件夹**页面上，使用“浏览”按钮选择导出的 VM 定义文件  
  
3.  在**选择虚拟机**页面上，单击源计算机。  
  
4.  在 **选择导入类型** 页面上，单击 **复制虚拟机（创建新的唯一 ID）** ，然后单击 **完成**。  
  
5.  如果在同一台 Hyper-V 主机上执行导入，则重命名导入的 VM；它将具有与导出的源域控制器相同的名称。  
  
![虚拟化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportLocateFolder.png)  
  
![虚拟化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportSelectVM.png)  
  
![虚拟化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportChooseType.gif)  
  
请记住，使用“Hyper-V 管理”管理单元删除任何已导入的快照：  
  
![虚拟化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportDelSnap.gif)  
  
> [!WARNING]  
> 删除任何已导入的快照至关重要；如果已应用，它们可将克隆的域控制器返回到之前的（可能是动态的）DC 的状态，从而导致复制失败、重复的 IP 信息和其他突发事件。  
  
##### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
使用以下 cmdlet，你可以在 Windows Server 2012 中使用 Hyper-V Windows PowerShell 模块自动化 VM 导入：  
  
```  
Import-VM  
Rename-VM  
```  
  
例如，下面使用其自动确定的 XML 文件导入已导出的 VM DC2-CLONED，然后立即将它重命名为其新的 VM 名称 DC5-CLONEDFROMDC2：  
  
![虚拟化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSImportVM.png)  
  
请记住，使用以下 cmdlet 删除任何已导入的快照：  
  
```  
Get-VMSnapshot  
Remove-VMSnapshot  
```  
  
例如：  
  
![虚拟化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSGetVMSnap.png)  
  
> [!WARNING]
> 确保导入计算机时，不会将静态 MAC 地址分配给源域控制器。 如果克隆具有静态 MAC 的源计算机，则这些复制的计算机将无法正确发送或接收任何网络流量。 如果出现这种情况，请设置新的唯一静态或动态 MAC 地址。 你可以看到 VM 是否通过该命令使用静态 MAC 地址：  
> 
> **VMName**   
>  ***测试-vm* |VMNetworkAdapter |fl \\** *  
  
### <a name="step-9---clone-the-new-virtual-machine"></a>步骤 9 – 克隆新的虚拟机  
（可选）在开始克隆之前，请重启脱机克隆源域控制器。 无论如何，请确保 PDC 模拟器处于联机状态。  
  
若要开始克隆，只需启动新的虚拟机。 该过程将自动启动，而且克隆完成后，域控制器将自动重新启动。  
  
> [!IMPORTANT]  
> 不建议使域控制器保持较长时间的关闭状态，如果克隆加入与其源 DC 相同的站点，则如果源域控制器处于脱机状态，可能会需要较长的时间生成初始站点内和站点间复制拓扑。  
  
如果使用 Windows PowerShell 启动 VM，则新的 Hyper-V 模块 cmdlet 为：  
  
```  
Start-VM  
```  
  
例如：  
  
![虚拟化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSStartVM.png)  
  
在克隆完成并重启计算机后，它将成为域控制器，你可以正常地登录以确认正常操作。 如果存在任何错误，则服务器将设置为在目录服务还原模式下启动以进行调查。  
  
## <a name="BKMK_VDCSafeRestore"></a>虚拟化安全措施  
与虚拟化域控制器克隆不同，Windows Server 2012 虚拟化安全措施无需执行任何配置步骤。 只要满足一些简单的条件，该功能即可不受干预地运行：  
  
-   虚拟机监控程序支持 VM 生成 ID  
  
-   存在一个有效的伙伴域控制器，还原的域控制器可从其中非权威地复制更改。  
  
### <a name="validate-the-hypervisor"></a>验证虚拟机监控程序  
通过查看供应商文档，确保源域控制器在支持的虚拟机监控程序上运行。 虚拟化域控制器与虚拟机监控程序无关，且不需要 Hyper-v。  
  
请查看之前的[平台要求](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_PlatformReqs)部分，以获取已知的 VM 生成 ID 支持。  
  
如果将 VM 从源虚拟机监控程序迁移到另一个目标虚拟机监控程序，则可能会也可能不会触发虚拟化安全措施，具体取决于虚拟机监控程序是否支持 VM 生成 ID，如下表所述。  
  
|源虚拟机监控程序|目标虚拟机监控程序|结果|  
|---------------------|---------------------|----------|  
|支持 VM 生成 ID|不支持 VM 生成 ID|未触发安全措施（如果存在 DCCloneConfigFile.xml，则 DC 将启动进入 DSRM）|  
|不支持 VM 生成 ID|支持 VM 生成 ID|已触发安全措施|  
|支持 VM 生成 ID|支持 VM 生成 ID|因为 VM 定义未发生更改，所以未触发安全措施，这意味着 VM 生成 ID 保持不变|  
  
### <a name="validate-the-replication-topology"></a>验证复制拓扑  
虚拟化安全措施将启动用于 Active Directory 复制的增量的非权威入站复制，以及所有 SYSVOL 内容的非权威重新同步。 这可确保域控制器从快照返回并具有完整功能，并且它最终与环境的其余部分保持一致。  
  
此新增功能附带几个要求和限制：  
  
-   还原的域控制器必须能够联系可写入的 DC  
  
-   绝不可同时还原一个域中的所有域控制器  
  
-   任何源于自从拍摄快照后尚未复制出站的已还原域控制器的更改都将永远丢失  
  
虽然疑难解答部分涉及了这些应用场景，但下面的详细信息将确保你不会创建可能引起问题的拓扑。  
  
#### <a name="writable-domain-controller-availability"></a>可写域控制器的可用性  
如果还原，则域控制器必须可连接到可写入的域控制器；只读域控制器无法发送更新的增量。 对此，拓扑很可能已经是正确的，因为可写入的域控制器始终需要可写入的伙伴。 但是，如果所有可写入的域控制器同时还原，则它们都无法找到有效的源。 如果可写入的域控制器因维护而处于脱机状态，或由于其他原因无法通过网络进行访问，则会发生相同的情况。  
  
#### <a name="simultaneous-restore"></a>同时还原  
请勿同时还原单个域中的所有域控制器。 如果所有快照同时还原，则 Active Directory 复制可正常工作，但 SYSVOL 复制会中止。 FRS 和 DFSR 的还原体系结构要求将其副本实例设置为非权威同步模式。 如果所有域控制器同时还原，并且每个域控制器对于 SYSVOL 将其本身标记为非权威，则之后它们都将尝试从权威的伙伴同步组策略和脚本；不过，此时所有伙伴也都是非权威伙伴。  
  
> [!IMPORTANT]  
> 如果同时还原所有域控制器，请使用以下文章将一台域控制器（通常为 PDC 模拟器）设置为权威，以便其他域控制器可以返回到正常操作：  
>   
> [使用 BurFlags 注册表项重新预置“文件复制服务”副本集](https://support.microsoft.com/kb/290762)  
>   
> [如何强制进行 DFSR 复制的 SYSVOL 的权威和非权威同步（如 FRS 的 "D4/D2"）](https://support.microsoft.com/kb/2218556)  
  
> [!WARNING]  
> 请勿在同一个虚拟机监控程序主机上的林或域中运行所有域控制器。 这样将引入单一故障点，每次虚拟机监控程序处于脱机状态时，该单一故障点都会使 AD DS、Exchange、SQL 和其他企业操作失败。 这无异于仅将一个域控制器用于一整个域或林。 多个平台上的多个域控制器有助于提供冗余和容错。  
  
#### <a name="post-snapshot-replication"></a>快照后复制  
在将创建快照后在本地进行的所有更改复制出站之前，请勿还原快照。 如果其他域控制器尚未通过复制接收它们，则产生的任何更改都将永远丢失。  
  
使用 Repadmin.exe 来显示域控制器及其伙伴之间的任何未复制的出站更改：  
  
1.  使用以下参数返回 DC 的伙伴名称和 DSA 对象 GUID：  
  
    ```  
    Repadmin.exe /showrepl <DC Name of the partner> /repsto  
    ```  
  
2.  将伙伴域控制器的挂起的入站复制返回到要还原的域控制器：  
  
    ```  
    Repadmin.exe /showchanges < Name of partner DC><DSA Object GUID of the domain controller being restored><naming context to compare>  
    ```  
  
或者，只查看未复制的更改的计数：  
  
```  
Repadmin.exe /showchanges <Name of partner DC><DSA Object GUID of the domain controller being restored><naming context to compare> /statistics  
```  
  
例如（已修改输出以提高可读性并对重要条目使用 ***斜体化***），下面你可看到 DC4 的复制伙伴关系：  
  
```  
C:\>repadmin.exe /showrepl dc4.corp.contoso.com /repsto  
  
Default-First-Site-Name\DC4  
DSA Options: IS_GC  
Site Options: (none)  
DSA object GUID: 5d083398-4bd3-48a4-a80d-fb2ebafb984f  
DSA invocationID: 730fafec-b6d4-4911-88f2-5b64e48fc2f1  
  
==== OUTBOUND NEIGHBORS FOR CHANGE NOTIFICATIONS ============  
  
DC=corp,DC=contoso,DC=com  
    Default-First-Site-Name\DC3 via RPC  
        DSA object GUID: f62978a8-fcf7-40b5-ac00-40aa9c4f5ad3  
        Last attempt @ 2011-11-11 15:04:12 was successful.  
    Default-First-Site-Name\DC2 via RPC  
        DSA object GUID: 3019137e-d223-4b62-baaa-e241a0c46a11  
        Last attempt @ 2011-11-11 15:04:15 was successful.  
```  
  
现在，你知道它使用 DC2 和 DC3 进行复制。 然后显示 DC2 声明它仍未从 DC4 获得的更改的列表，你将看到存在一个新的组：  
  
```  
C:\>repadmin /showchanges dc2.corp.contoso.com 5d083398-4bd3-48a4-a80d-fb2ebafb984f dc=corp,dc=contoso,dc=com  
  
==== SOURCE DSA: (null) ====  
Objects returned: 1  
(0) add CN=newgroup4,CN=Users,DC=corp,DC=contoso,DC=com  
    1> parentGUID: 55fc995a-04f4-4774-b076-d6a48ac1af99  
    1> objectGUID: 96b848a2-df1d-433c-a645-956cfbf44086  
    2> objectClass: top; group  
    1> instanceType: 0x4 = ( WRITE )  
    1> whenCreated: 11/11/2011 3:03:57 PM Eastern Standard Time  
```  
  
你还将测试其他伙伴，以确保它尚未复制。  
  
或者，如果你并不关心哪些对象并未复制，而仅关心任何对象是否未完成，你可以使用 **/statistics** 选项：  
  
```  
C:\>repadmin /showchanges dc2.corp.contoso.com 5d083398-4bd3-48a4-a80d-fb2ebafb984f dc=corp,dc=contoso,dc=com /statistics  
  
***********************************************  
********* Grand total *************************  
Packets:              1  
Objects:              1Object Additions:     1Object Modifications: 0Object Deletions:     0Object Moves:         0Attributes:           12Values:               13  
```  
  
> [!IMPORTANT]  
> 如果你看到任何失败或未完成的复制，请测试所有可写入的伙伴。 只要聚合至少一个可写入的伙伴，通常可以安全地还原快照，因为最终可传递复制会协调其他服务器。  
>   
> 请务必注意由 /showchanges 所示的复制中的任何错误，而且在修复这些错误之前，请勿继续。  
  
### <a name="windows-powershell-snapshot-cmdlets"></a>Windows PowerShell 快照 Cmdlet  
以下 Windows PowerShell Hyper-V 模块 cmdlet 提供了 Windows Server 2012 中的快照功能：  
  
```  
Checkpoint-VM  
Export-VMSnapshot  
Get-VMSnapshot  
Remove-VMSnapshot  
Rename-VMSnapshot  
Restore-VMSnapshot  
```  
  


