---
ms.assetid: b146f47e-3081-4c8e-bf68-d0f993564db2
title: "虚拟化的域控制器部署和配置"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: dcb377cef003458bcdf2e4b3564167cee4310020
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="virtualized-domain-controller-deployment-and-configuration"></a>虚拟化的域控制器部署和配置

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本主题介绍：  
  
-   [安装注意事项](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_InstallConsiderations)  
  
    这包括平台要求和其他重要约束。  
  
-   [虚拟化域控制器复制](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_VDCCloning)  
  
    这详细说明整个虚拟化的域控制器复制过程。  
  
-   [虚拟化的域控制器安全还原](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_VDCSafeRestore)  
  
    这将详细说明期间虚拟化的域控制器安全还原进行的验证。  
  
## <a name="BKMK_InstallConsiderations"></a>安装注意事项  
没有特殊角色或功能安装虚拟化的域控制器;所有控制器自动都包含克隆和安全还原功能。 你无法删除或禁用这些功能。  
  
使用 Windows Server 2012 域控制器需要一个 Windows Server 2012 广告 DS 方案 56 或更高版本和 Windows Server 2003 本机等于或更高版本森林功能级别。  
  
全球目录和 FSMO 角色一样，这两个可写和只读域控制器支持虚拟化直流的所有方面。  
  
> [!IMPORTANT]  
> 复制开始时，必须联机 PDC 仿真器 FSMO 角色所有者。  
  
### <a name="BKMK_PlatformReqs"></a>平台要求  
虚拟化域控制器复制要求：  
  
-   Windows Server 2012 DC 上托管 PDC 模拟器 FSMO 角色  
  
-   复制操作期间的可用 PDC 仿真器  
  
还原克隆和安全要求：  
  
-   Windows Server 2012 虚拟化来宾  
  
-   虚拟化主机平台支持 VM 代 ID (VMGID)  
  
检查下表中的虚拟化产品以及他们所支持是否虚拟化域控制器和 VM 代 id。  
  
|||  
|-|-|  
|**虚拟化产品**|**支持虚拟化的域控制器和 VMGID**|  
|**Microsoft Windows Server 2012 服务器 HYPER-V 功能**|是的|  
|**Microsoft Windows Server 2012 Hyper V 服务器**|是的|  
|**Microsoft Windows Hyper V 的客户端 8 功能**|是的|  
|**Windows Server 2008 R2 和 Windows Server 2008**|不|  
|**非 Microsoft 虚拟化解决方案**|请联系供应商|  
  
即使 Microsoft 支持 Windows 7 Virtual PC、 虚拟电脑 2007年、 虚拟电脑 2004 年和虚拟 Server 2005，它们不能运行 64 位来宾，也不支持 VM GenerationID。  
  
对于第三方虚拟化产品的帮助和与虚拟化的域控制器其支持姿态，请直接联系该供应商。  
  
有关详细信息，查看有关支持策略[非 Microsoft 硬件虚拟化软件上运行的 Microsoft 软件](https://support.microsoft.com/kb/897615)。  
  
### <a name="critical-caveats"></a>关键的几点注意事项  
虚拟化的域控制器执行*不*支持安全还原项操作：  
  
-   VHD 和 VHDX 手动将覆盖现有 VHD 文件复制的文件  
  
-   使用文件备份或全部磁盘备份软件还原 VHD 和 VHDX 文件  
  
> [!NOTE]  
> 对 Windows Server 2012 HYPER-V 新均 VHDX 文件。  
  
这些操作都不属于 VM GenerationID 语义，因此不会更改 VM 代 id。 还原的域控制器使用以下方法可能会导致 USN 回退，然后在隔离域控制器，或者介绍延迟对象和需森林宽清理操作。  
  
> [!WARNING]  
> 虚拟化的域控制器安全还原不备份系统状态和广告 DS 回收站替代。  
>   
> 还原快照之后, 的以前未复制更改后快照源自该域控制器增量都将被永久。 安全还原实现自动的未授权还原为了防止意外域控制器隔离*仅*。  
  
有关 USN 气泡和延迟对象的详细信息，请参阅[失败，错误 8606 的疑难解答 Active Directory 操作:"不足属性被给创建一个"](https://support.microsoft.com/kb/2028495)。  
  
## <a name="BKMK_VDCCloning"></a>虚拟化域控制器复制  
有大量阶段和到复制虚拟化的域控制器，无论是使用的图形工具或 Windows PowerShell 的步骤。 在较高级别的三个阶段如下：  
  
**准备环境**  
  
-   第 1 步： 验证虚拟机监控程序支持 VM 代 ID，并因此，复制  
  
-   第 2 步： 验证在复制 PDC 模拟器角色托管由域控制器，运行 Windows Server 2012 并且它处于联机状态并且可以访问由复制的域控制器。  
  
**准备源域控制器**  
  
-   第 3 步： 授权复制的源代码域控制器  
  
-   第 4 步： 删除不兼容的服务或应用程序，或将其添加到 CustomDCCloneAllowList.xml 文件。  
  
-   步骤 5： 创建 DCCloneConfig.xml  
  
-   第 6 步： 脱机源域控制器  
  
**创建复制的域控制器**  
  
-   第 7 步： 复制或导出源 VM，并添加 XML 如果尚未复制  
  
-   第 8 步： 复制从创建新的虚拟机  
  
-   第 9 步： 启动新的虚拟机，若要开始复制  
  
使用 HYPER-V 管理控制台如图形工具或命令行工具，如 Windows PowerShell 与这两个接口一次提供相关步骤以便时会该操作无过程的差异。 本主题提供了一些供你探索克隆过程; 的端到端自动化的 Windows PowerShell 示例他们并不需要的任何步骤进行操作。 没有为包含在 Windows Server 2012 的虚拟化的域控制器图形管理工具。  
  
几个要点在的过程中可有如何创建复制的计算机和您如何添加 xml 文件; 选择在下面的详细信息，这些步骤会标注。 否则，该进程是不可更改。  
  
下图说明虚拟化的域控制器克隆过程，域已经存在。  
  
![虚拟化的 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_CloningProcessFlow.png)  
  
### <a name="step-1---validate-the-hypervisor"></a>第 1 步-验证虚拟机监控程序  
请确保源域控制器上受支持的虚拟机监控程序运行通过查看供应商文档。 虚拟化的域控制器虚拟机监控程序独立于，并不需要 HYPER-V。  
  
Microsoft HYPER-V 虚拟机监控程序是否，请确保它运行的 Windows Server 2012。 你可以验证此使用设备管理  
  
打开**Devmgmt.msc**并检查**系统设备**安装 Microsoft HYPER-V 设备和驱动程序。 系统特定设备所需的虚拟化的域控制器**Microsoft HYPER-V 代计数器**(驱动程序： vmgencounter.sys)。  
  
![虚拟化的 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVVMGenIDCounter.png)  
  
### <a name="step-2---verify-the-pdce-fsmo-role"></a>第 2 步-确认 PDCE FSMO 角色  
在尝试复制 DC 之前，你必须验证主持主要域控制器仿真器 FSMO 的域控制器运行 Windows Server 2012。 有几个原因是必需的 PDC 仿真器 (PDCE):  
  
1.  PDCE 创建特殊**克隆域控制器**分组，并允许域控制器要复制本身的域根上设置其权限。  
  
2.  克隆域控制器联系人 PDCE 直接使用 DRSUAPI RPC 协议，，以便创建副本 DC 计算机对象。  
  
    > [!NOTE]  
    > Windows Server 2012 扩展现有的目录复制服务 (DRS) 远程协议 (UUID **E3514235-4B06-11D1-AB04-00C04FC2DCD2**) 包含一个新 RPC 方法**IDL_DRSAddCloneDC** (Opnum **28**)。 **IDL_DRSAddCloneDC**方法创建新的域控制器对象，方法是从现有域控制器对象复制属性。  
    >   
    > 计算机、 server、 NTDS 设置、 FRS、 DFSR 和连接对象保留每个域控制器由域控制器的状态。 复制对象时, 此 RPC 方法替换为原来的域控制器的所有参考相应的新的域控制器的对象。 呼叫必须控件的访问权限 DS-克隆-域控制器上的域命名上下文。  
    >   
    > 使用此新的方法始终需要直接访问 PDC 仿真器域控制器来自调用方。  
    >   
    > 由于此 RPC 方法是新，你的网络分析软件要求更新的分析器包含的新 Opnum 28 日，现有 UUID E3514235 4B06-11 维 1-AB04-00C04FC2DCD2 字段。 否则，你无法分析此交通。  
    >   
    > 有关详细信息，请参阅[4.1.29 IDL_DRSAddCloneDC (Opnum 28)](https://msdn.microsoft.com/en-us/library/hh554213(v=prot.13).aspx)。  
  
***这也意味着时使用非完全路由的网络虚拟化的域控制器复制需要有权访问 PDCE 网络段***。 它是接受后，只要你是留意更新广告 DS 逻辑站点信息复制-就像物理域控制器-将复制的域控制器移动到不同的网络。  
  
> [!IMPORTANT]  
> 复制包含一个域控制器某个域，当你必须确保源 DC 重新联机先克隆副本。 生产域始终应包含至少两个域控制器。  
  
#### <a name="active-directory-users-and-computers-method"></a>Active Directory 的用户和计算机的方法  
  
1.  通过使用 Dsa.msc 贴靠，右键单击域，然后单击**操作主机**。 请注意名为 PDC 选项卡上的域控制器，并关闭对话框。  
  
2.  右键单击该直流计算机对象，然后单击**属性**，然后验证操作系统的信息。  
  
#### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
你可以组合下面的 Active Directory 的 Windows PowerShell 模块 cmdlet 返回 PDC 模拟器版本：  
  
```  
Get-adddomaincontroller  
Get-adcomputer  
```  
  
如果未提供域，这些 cmdlet 假定计算机运行的位置的域。  
  
以下命令返回 PDCE 和操作系统的信息：  
  
```  
get-adcomputer(Get-ADDomainController -Discover -Service "PrimaryDC").name -property * | format-list dnshostname,operatingsystem,operatingsystemversion  
```  
  
此下面的示例演示指定域名和筛选返回之前的 Windows PowerShell 管道属性：  
  
![虚拟化的 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PDCOSInfo.png)  
  
### <a name="step-3---authorize-a-source-dc"></a>第 3 步-授权源 DC  
源域控制器必须控制访问权限 （汽车）**允许创建自身的副本 DC**上的域 NC 头。 默认情况下，已知的组**克隆域控制器**具有此权限，并且不包含成员。 在 Windows Server 2012 域控制器传输该 FSMO 角色，PDCE 创建此组。  
  
#### <a name="active-directory-administrative-center-method"></a>Active Directory 管理中心方法  
  
1.  开始 Dsac.exe 并导航到源直流，然后打开其详细信息页面。  
  
2.  在**隶属于**部分中，添加**克隆域控制器**该域组。  
  
#### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
你可以将组合以下 Active Directory 的 Windows PowerShell 模块 cmdlet**获取 adcomputer**和**添加 adgroupmember**添加到域控制器**克隆域控制器**组：  
  
```  
Get-adcomputer <dc name> | %{add-adgroupmember "cloneable domain controllers" $_.samaccountname}  
```  
  
例如，这将服务器 DC1 添加到组中，而无需中指定的组成员辨别的名称：  
  
![虚拟化的 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_AddDcToGroup.png)  
  
#### <a name="rebuilding-default-permissions"></a>重新生成默认的权限  
如果你从域头删除此权限，复制无法正常工作。 你可以重新创建使用 Active Directory 管理中心或 Windows PowerShell 的权限。  
  
##### <a name="active-directory-administrative-center-method"></a>Active Directory 管理中心方法  
  
1.  打开**Active Directory 管理中心**、 右键单击域头、 单击**属性**，单击**扩展**选项卡上，单击**安全**，然后单击**高级**。 单击**只是这个对象**。  
  
2.  单击**添加**下**输入要选择的对象名称**，键入组的名称**克隆域控制器。**  
  
3.  在权限中，单击**允许 DC 创建自身的副本**，然后单击**确定**。  
  
> [!NOTE]  
> 你还可以删除默认权限并添加个人域控制器。 这种可能导致持续维护问题，但是，此自定义的不知道新管理员所在。 更改默认设置不会增加安全和建议不要。  
  
##### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
使用以下命令，在管理员提升 Windows PowerShell 控制台提示。 检测域名这些命令，并添加重新中默认的权限：  
  
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
  
或者，运行示例[FixVDCPermissions.ps1](../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_FixPDCPerms)在 Windows PowerShell 控制台中，控制台作为提升管理员受影响的域中的域控制器上的开始位置。 它会自动设置的权限。 在此模块附录位于示例。  
  
### <a name="step-4---remove-incompatible-applications-or-services-if-not-using-customdccloneallowlistxml"></a>第 4 步-删除不兼容的应用程序或服务 （如果使用的不 CustomDCCloneAllowList.xml）  
所有程序或服务获取的 ADDCCloningExcludedApplicationList-之前返回*，未添加到 CustomDCCloneAllowList.xml* -复制之前，必须先删除。 卸载该应用或服务是推荐的方法。  
  
> [!WARNING]  
> 任何不兼容的程序或服务无法卸载或添加到 CustomDCCloneAllowList.xml 阻止复制。  
  
使用获取 AdComputerServiceAccount cmdlet 在域中查找所有独立托管服务帐户 (Msa) 并且如果此计算机使用其中任一。 如果安装了任何 MSA，可使用卸载 ADServiceAccount cmdlet 删除本地安装的服务的帐户。 一旦你完成在第 6 步拍摄源域控制器离线，你可以重新添加到服务器重新联机时，使用安装 ADServiceAccount 的 MSA。 有关详细信息，请参阅[卸载 ADServiceAccount](https://technet.microsoft.com/en-us/library/hh852310)。  
  
> [!IMPORTANT]  
> -第一次发布了 Windows Server 2008 R2-独立 Msa 已更换组 Msa 与 Windows Server 2012。 组的 Msa 支持复制。  
  
### <a name="step-5---create-dccloneconfigxml"></a>第 5 步-创建 DCCloneConfig.xml  
DcCloneConfig.xml 文件时需要复制域控制器。 它的内容，可以指定唯一的详细信息，如新的计算机名称和 IP 地址。  
  
CustomDCCloneAllowList.xml 文件是可选的除非你安装的应用程序或潜在不兼容 Windows 服务源域控制器。 这些文件需要精确命名、 格式设置和位置;否则，复制无法正常工作。  
  
出于该原因，你应始终使用 Windows PowerShell cmdlet 创建 XML 文件并将其放在正确的位置。  
  
#### <a name="generating-with-new-addccloneconfigfile"></a>使用新 ADDCCloneConfigFile 生成  
Active Directory 的 Windows PowerShell 模块包含在 Windows Server 2012 新 cmdlet:  
  
```  
New-ADDCCloneConfigFile  
```  
  
你想要复制的建议的源域控制器上运行 cmdlet。 Cmdlet 支持多个参数，并且在使用时，始终测试计算机和环境，除非你指定的运行位置-脱机参数。  
  
||||  
|-|-|-|  
|**与 active Directory**<br /><br />**Cmdlet**|**参数**|**说明**|  
|**新 ADDCCloneConfigFile**|*<no argument specified>*|DSA 工作目录中创建一个空白 DcCloneConfig.xml 文件 (默认: %systemroot%\ntds)|  
||-CloneComputerName|指定副本 DC 计算机名称。 字符串数据类型。|  
||-路径|指定创建 DcCloneConfig.xml 的文件夹。 如果未指定，写入 DSA 工作目录 (默认: %systemroot%\ntds)。 字符串数据类型。|  
||-站点名|指定加入复制的计算机帐户创建期间的广告逻辑站点名称。 字符串数据类型。|  
||-IPv4Address|指定复制计算机的静态 IPv4 地址。 字符串数据类型。|  
||-IPv4SubnetMask|指定静态 IPv4 子网掩码复制的计算机。 字符串数据类型。|  
||-IPv4DefaultGateway|指定复制计算机的静态 IPv4 默认网关地址。 字符串数据类型。|  
||-IPv4DNSResolver|用逗号分隔列表中指定复制计算机静态 IPv4 DNS 的条目。 很好地满足数据类型。 可以提供最多四个条目。|  
||-PreferredWINSServer|指定主 WINS 服务器的静态 IPv4 的地址。 字符串数据类型。|  
||-AlternateWINSServer|指定辅助 WINS 服务器的静态 IPv4 地址。 字符串数据类型。|  
||-IPv6DNSResolver|用逗号分隔列表中指定复制计算机静态 IPv6 DNS 的条目。 没有任何方法可设置中虚拟化的域控制器复制 Ipv6 静态信息。 很好地满足数据类型。|  
||-离线|不会执行的验证测试，并将覆盖现有的任何 dccloneconfig.xml。 没有任何参数。 有关详细信息，请参阅[在离线模式下运行的新 ADDCCloneConfigFile](../../../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#BKMK_OfflineMode)。|  
||*静态*|如果指定静态 IP 参数 IPv4SubnetMask、 IPv4SubnetMask 或 IPv4DefaultGateway，必需。 没有任何参数。|  
  
在联机模式下运行时执行测试：  
  
-   PDC 模拟器是 Windows Server 2012 或更高版本  
  
-   源域控制器是克隆域控制器组成员  
  
-   源域控制器不包括任何排除应用程序或服务  
  
-   源域控制器不包含 DcCloneConfig.xml 指定路径  
  
![虚拟化的 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSNewDCCloneConfig.png)  
  
### <a name="step-6---take-the-source-domain-controller-offline"></a>第 6 步-使源域控制器离线  
你无法将正在运行的源直流; 复制它完美必须关机。 不要不复制停止 graceless 断电了域控制器。  
  
#### <a name="graphical-method"></a>图形的方法  
使用运行直流，在关闭按钮或 HYPER-V 管理器关闭按钮。  
  
![虚拟化的 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_Shutdown.png)  
  
![虚拟化的 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVShutdown.png)  
  
#### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
你可以关闭虚拟机，使用以下 cmdlet 之一：  
  
```  
Stop-computer  
Stop-vm  
```  
  
停止计算机是 cmdlet 支持关机虚拟化，无论的计算机，并且类似于旧版 Shutdown.exe 实用程序。 停止 vm 新 cmdlet 在 Windows Server 2012 HYPER-V Windows PowerShell 模块中，并且相当于 HYPER-V 管理器中的电源选项。 后者非常有用的域控制器在通常表示虚拟专用网络的实验环境中。  
  
![虚拟化的 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_StopComputer2.png)  
  
![虚拟化的 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_StopVM.png)  
  
### <a name="step-7---copy-disks"></a>步骤 7-复制的磁盘  
管理选择中复制阶段需要：  
  
-   复制的磁盘手动，无 HYPER-V  
  
-   导出 VM 中，使用 HYPER-V  
  
-   导出合并的磁盘，使用 HYPER-V  
  
必须将所有虚拟机的磁盘复制，而不仅仅是系统驱动器。 如果你计划移到另一台 HYPER-V 主机复制的域控制器源域控制器使用差异磁盘，你必须导出。  
  
如果仅有源域控制器手动复制的磁盘建议*一个*驱动器。 对于虚拟机的功能与建议导出月导入*多个*驱动器或其他复杂的虚拟化的硬件自定义等多个 Nic。  
  
如果文件复制手动，删除任何前复制的快照。 如果要导出 VM，删除前导出快照或后导入的新虚拟机从删除它们。  
  
> [!WARNING]  
> 快照有差异可以返回到以前的状态域控制器的磁盘。 如果你要复制的域控制器，然后还原其预克隆的快照，你会结束森林中的重复域控制器。 没有值在之前的快照新复制的域控制器上。  
  
#### <a name="manually-copying-disks"></a>手动复制的磁盘  
  
##### <a name="hyper-v-manager-method"></a>管理器中 hyper V 的方法  
使用 HYPER-V 管理器贴靠中确定哪些磁盘源域控制器与相关。 检查选项用于验证域控制器使用差异磁盘 （这需要还复制家长磁盘）  
  
![虚拟化的 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVInspect.png)  
  
若要删除的快照，选择一个虚拟机，并删除快照树。  
  
![虚拟化的 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVDeleteSnapshot.gif)  
  
然后，你可以手动复制使用 Windows 资源管理器、 Xcopy.exe 或 Robocopy.exe VHD 或 VHDX 文件。 没有特殊的步骤都需要。 若要更改的文件名，即使移到另一个文件夹最佳做法是。  
  
> [!NOTE]  
> 如果 LAN 上的主计算机之间复制 (1 Gb 或更高版本)，则**Xcopy.exe /J**选项可复制 VHD/VHDX 文件其他工具，但这会损失多更高带宽使用量比相当更快。  
  
##### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
若要确定使用的 Windows PowerShell 的磁盘，请使用 HYPER-V 模块:  
  
```  
Get-vmidecontroller  
Get-vmscsicontroller  
Get-vmfibrechannelhba  
Get-vmharddiskdrive  
```  
  
例如，你可以从 VM 命名返回所有 IDE 硬盘**DC2**使用下面的示例：  
  
![虚拟化的 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_ReturnIDE.png)  
  
如果磁盘路径指向 AVHD 或 AVHDX 的文件，它是快照。 若要删除的磁盘和中的实际 VHD 或 VHDX 合并关联的快照，使用 cmdlet:  
  
```  
Get-VMSnapshot  
Remove-VMSnapshot  
```  
  
例如，若要从 VM 中删除所有快照名为 DC2 SOURCECLONE:  
  
![虚拟化的 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_DelSnapshots.png)  
  
若要复制的文件，使用 Windows PowerShell，使用以下 cmdlet:  
  
```  
Copy-Item  
```  
  
将与中管道以帮助自动化 VM cmdlet 组合。 管道是使用多个 cmdlet 来通过数据之间的频道。 例如，要复制的离线状态下的源域控制器驱动器名为新磁盘称为而无需知道确切的路径其系统驱动器 c:\temp\copy.vhd 为 DC2 SOURCECLONE:  
  
```  
Get-VMIdeController dc2-sourceclone | Get-VMHardDiskDrive | select-Object {copy-item -path $_.path -destination c:\temp\copy.vhd}  
```  
  
![虚拟化的 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSCopyDrive.png)  
  
> [!IMPORTANT]  
> 不能使用层通过磁盘复制，因为它们不使用虚拟磁盘文件而实际硬盘。  
  
> [!NOTE]  
> 有关更多的 Windows PowerShell 操作管道的详细信息，请参阅[管道，并且在 Windows PowerShell 管道](https://technet.microsoft.com/en-us/library/ee176927.aspx)。  
  
#### <a name="exporting-the-vm"></a>导出 VM  
作为替代复制的磁盘，你可以将整个 HYPER-V VM 导出作为副本。 自动导出创建文件夹 VM 的名为并包含所有磁盘和配置的信息。  
  
![虚拟化的 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVExport.png)  
  
##### <a name="hyper-v-manager-method"></a>管理器中 hyper V 的方法  
若要导出与 HYPER-V 经理 VM:  
  
1.  右键单击源域控制器，然后单击**导出**。  
  
2.  作为导出容器选择现有的文件夹。  
  
3.  等待状态栏停止显示**导出**。  
  
##### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
若要导出 VM HYPER-V Windows PowerShell 模块的使用，使用 cmdlet:  
  
```  
Export-vm  
```  
  
例如，VM 中导出到一个名为 C:\VM 文件夹命名 DC2 SOURCECLONE:  
  
![虚拟化的 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSExport.png)  
  
> [!NOTE]  
> Windows Server 2012 HYPER-V 支持新导出和导入此培训范围之外的功能。 有关详细信息，查看 TechNet。  
  
#### <a name="exporting-merged-disks-using-hyper-v"></a>导出合并的磁盘，使用 HYPER-V  
最后一个选项是使用 HYPER-V 内的磁盘合并及转换选项。 这些允许你以一份现有的磁盘结构-即使包括快照 AVHD/AVHDX 文件-转化为一个新的磁盘。 手动磁盘复制方案，如此主要适用于更易于使用仅使用单张驱动器，如 C:\\ 的虚拟机。 它独立优点是，手动与复制不同，它不需要你第一次删除的快照。 此操作时比只需删除快照并复制的磁盘一定慢。  
  
##### <a name="hyper-v-manager-method"></a>管理器中 hyper V 的方法  
若要创建合并的磁盘使用 HYPER-V 管理器：  
  
1.  单击**编辑磁盘**。  
  
2.  浏览最低孩子磁盘。 例如，如果你使用的差异的磁盘，孩子磁盘是最小的孩子。 如果有虚拟机的快照 （或多个），当前所选的快照是最低孩子磁盘。  
  
3.  选择**合并**选项来创建一个磁盘退出整个家长孩子结构。  
  
4.  选择新的虚拟硬盘并提供一个路径。 这不是在还原以前的快照风险一个新便携设备插入调节现有 VHD/VHDX 文件。  
  
##### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
若要创建一组复杂使用 HYPER-V Windows PowerShell 模块的家长合并的磁盘，使用 cmdlet:  
  
```  
Convert-vm  
```  
  
例如，要导出的虚拟机的磁盘快照 （不包括任何差异磁盘此时间） 整个链和转化为新的单个磁盘父磁盘名为 DC4 复制。VHDX:  
  
![虚拟化的 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSConvertVhd.png)  
  
#### <a name="BKMK_Offline"></a>脱机系统的磁盘中添加 XML  
如果你未将 Dccloneconfig.xml 复制到运行源直流，必须现在将更新的 dccloneconfig.xml 文件复制到复制导出的离线系统的磁盘。 根据之前获取 ADDCCloningExcludedApplicationList 使用检测到已安装的应用程序，你可能还需要 CustomDCCloneAllowList.xml 文件复制到磁盘。  
  
在以下位置可能包含 DcCloneConfig.xml 文件：  
  
1.  工作目录 DSA  
  
2.  %windir%\NTDS  
  
3.  可移动读取/写入媒体、的驱动器号，根部驱动器的订单  
  
这些途径不可配置。 复制开始之后，克隆检查该特定订单和使用首 DcCloneConfig.xml 这些位置的文件找到，无论该文件夹的内容。  
  
在以下位置可能包含 CustomDCCloneAllowList.xml 文件：  
  
1.  HKey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters  
  
    AllowListFolder (*REG_SZ*)  
  
2.  工作目录 DSA  
  
3.  %windir%\NTDS  
  
4.  可移动读取/写入媒体、的驱动器号，根部驱动器的订单  
  
你可以运行与的新 ADDCCloneConfigFile **-脱机**参数 （也称为离线模式） 创建 DcCloneConfig.xml 文件并将其放置在正确的位置。 下面的示例显示了如何在离线模式下运行新建 ADDCCloneConfigFile。  
  
若要创建克隆域控制器在离线模式下，在站点称为"REDMOND"静态 IPv4 地址，命名 CloneDC1 键入：  
  
```  
New-ADDCCloneConfigFile -Offline -CloneComputerName CloneDC1 -SiteName REDMOND -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -Static -Path F:\Windows\NTDS  
```  
  
若要创建克隆域控制器名称 Clone2 离线模式静态 IPv4 和静态 IPv6 设置中，键入：  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone2" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -Path F:\Windows\NTDS  
```  
  
若要指定 DNS 解析程序设置多个 DNS 服务器静态 IPv4 和动态 IPv6 设置离线模式中创建克隆域控制器，键入：  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.10" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -IPv4DNSResolver @( "10.0.0.1","10.0.0.2" ) -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS   
```  
  
若要创建克隆域控制器名称 Clone1 离线模式动态 IPv4 和静态 IPv6 设置中，键入：  
  
```  
New-ADDCCloneConfigFile -Offline -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone1" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -SiteName "REDMOND" -Path F:\Windows\NTDS  
```  
  
若要创建克隆域控制器在离线模式下，与动态 IPv4 和动态 IPv6 设置中，键入：  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4DNSResolver "10.0.0.1" -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS  
  
```  
  
##### <a name="windows-explorer-method"></a>Windows 资源管理器方法  
Windows Server 2012 现在提供图形安装 VHD 和 VHDX 文件所需的选项。 这将需要安装 Windows Server 2012 上的桌面体验功能。  
  
1.  单击新复制的 VHD/VHDX 文件包含源 DC 的系统驱动器或 DSA 工作目录的位置文件夹，，然后单击**装载**从**光盘图像工具**菜单。  
  
2.  现在戴式驱动器中，将 XML 文件复制到一个有效的位置。 你可能会提示您对该文件夹的权限。  
  
3.  单击该驱动器，单击**弹出**从**磁盘工具**菜单。  
  
![虚拟化的 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVClickMountedDrive.png)  
  
![虚拟化的 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVDetailsMountedDrive.gif)  
  
![虚拟化的 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVEjectMountedDrive.gif)  
  
##### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
或者，你可以装载离线状态下的磁盘，并将使用 Windows PowerShell cmdlet XML 文件复制：  
  
```  
mount-vhd  
get-disk  
get-partition  
get-volume  
Add-PartitionAccessPath  
Copy-Item  
```  
  
这允许你控制过程的完成。 例如，驱动器可装入与特定的驱动器号，复制该文件，并驱动器被卸载。  
  
```  
mount-vhd <disk path> -passthru -nodriveletter | get-disk | get -partition | get-volume | get-partition | where {$_.partition number -eq 2} | Add-PartitionAccessPath -accesspath <drive letter>  
  
copy-item <xml file path><destination path>\dccloneconfig.xml  
  
dismount-vhd <disk path>  
```  
  
例如：  
  
![虚拟化的 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSMountVHD.png)  
  
或者，你可以使用新**装载 DiskImage** cmdlet 装载 VHD （或 ISO） 的文件。  
  
### <a name="step-8---create-the-new-virtual-machine"></a>第 8 步-创建新的虚拟机  
在启动克隆进程之前最终配置步骤创建新的虚拟机使用复制的源域控制器中的磁盘。 根据所选内容复制的磁盘阶段内所做，你将有两个选项：  
  
1.  复制的磁盘相关联的新虚拟机  
  
2.  导入的已导出的 VM  
  
#### <a name="associating-a-new-vm-with-copied-disks"></a>将新 VM 与复制的磁盘相关联。  
如果你手动复制系统的磁盘，必须创建一个新的虚拟机使用复制的磁盘。 虚拟机监控程序自动设置 VM 代 ID 时创建一个新 VM;不配置需要更改在 VM 或 HYPER-V 主机。  
  
![虚拟化的 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVConnectVHD.gif)  
  
##### <a name="hyper-v-manager-method"></a>管理器中 hyper V 的方法  
  
1.  创建新的虚拟机。  
  
2.  指定 VM 名称、 内存和网络。  
  
3.  在连接虚拟硬盘页上，指定复制的系统的磁盘。  
  
4.  完成向导后，以创建 VM。  
  
如果我们进行了多个磁盘，网络适配器或其他自定义设置，它们在启动之前配置域控制器。 对于复杂虚拟机的功能，建议"导出导入"付款方式复制的磁盘。  
  
##### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
可以使用 HYPER-V Windows PowerShell 模块自动 VM 创建中使用以下 cmdlet 的 Windows Server 2012:  
  
```  
New-VM  
```  
  
例如，在此处 DC4 CLONEDFROMDC2 VM，使用创建 1 GB RAM，启动从 c:\vm\dc4-systemdrive-clonedfromdc2.vhd 文件，并使用 10.0 虚拟网络：  
  
![虚拟化的 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSNewVM.png)  
  
#### <a name="import-vm"></a>导入 VM  
如果你以前导出你 VM，你现在需要将其导入重新作为副本。 这只会消耗的已导出的 XML 创建所有以前的设置、 驱动器、 网络和设置内存使用的计算机。  
  
如果你打算从相同的已导出 VM 创建其他副本，可根据需要进行已导出 VM 多个的副本。 然后将每个副本导入。  
  
> [!IMPORTANT]  
> 请务必使用**副本**选项，因为导出保留源; 中的所有信息导入的服务器**移动**或**就地**导致信息碰撞如果相同 HYPER-V 主服务器上执行此操作。  
  
##### <a name="hyper-v-manager-method"></a>管理器中 hyper V 的方法  
若要导入使用 HYPER-V 管理器贴靠：  
  
1.  单击**导入虚拟机**  
  
2.  在**找到文件夹**页上，选择使用浏览按钮的已导出的 VM 定义文件  
  
3.  在**选择虚拟机**页上，单击计算机源。  
  
4.  在**选择导入类型**页上，单击**复制虚拟机 （创建新的唯一 ID）**，然后单击**完成**。  
  
5.  如果在相同的 HYPER-V 主机; 导入重命名导入的 VM它也将具有相同名称的已导出的源域控制器。  
  
![虚拟化的 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportLocateFolder.png)  
  
![虚拟化的 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportSelectVM.png)  
  
![虚拟化的 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportChooseType.gif)  
  
请记住，若要删除任何导入的快照，使用 HYPER-V 管理贴靠中：  
  
![虚拟化的 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportDelSnap.gif)  
  
> [!WARNING]  
> 删除任何导入的快照方式是非常重要;如果应用，它们会返回复制的域控制器的状态以前-和活动可能会-DC，导致复制故障、 IP 的重复信息，以及其他中断。  
  
##### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
可以使用 HYPER-V Windows PowerShell 模块自动 VM 导入正在使用以下 cmdlet 的 Windows Server 2012:  
  
```  
Import-VM  
Rename-VM  
```  
  
例如，下面是导出 VM DC2 复制使用它自动决定的 XML 文件，然后重命名为其新 VM 名称 DC5 CLONEDFROMDC2 的立即导入：  
  
![虚拟化的 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSImportVM.png)  
  
请记住，若要删除任何导入的快照，使用以下 cmdlet:  
  
```  
Get-VMSnapshot  
Remove-VMSnapshot  
```  
  
例如：  
  
![虚拟化的 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSGetVMSnap.png)  
  
> [!WARNING]  
> 确保，当导入计算机，静态的 MAC 地址有未分配给源域控制器。 如果复制静态 MAC 源的计算机，这些复制的计算机未正确发送或接收的任何网络流量。 如果出现这种情况，请设置新唯一静态或动态的 MAC 地址。 你可以看到 VM 的命令将使用静态的 MAC 地址：  
>   
> **获取 VM VMName**   
>  ***测试 vm* |获取 VMNetworkAdapter |佛罗里达 \ ***  
  
### <a name="step-9---clone-the-new-virtual-machine"></a>第 9 步-复制的新虚拟机  
（可选） 复制之前，请重启脱机克隆源域控制器。 请确保该来访 PDC 模拟器处于联机状态。  
  
若要开始复制，只需启动新的虚拟机。 该过程将自动启动，域控制器将自动重新启动后复制已完成。  
  
> [!IMPORTANT]  
> 不建议保持关闭长一段时间的域控制器，克隆加入与其源 DC 相同的网站时，如果初始内和间站点复制拓扑可能需要更长的时间版本如果源域控制器处于离线状态。  
  
如果使用的 Windows PowerShell 开始 VM，新的 HYPER-V 模块 cmdlet 是：  
  
```  
Start-VM  
```  
  
例如：  
  
![虚拟化的 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSStartVM.png)  
  
在计算机重启后复制完成后，域控制器且，您可以登录上正常以确认正常操作。 如果有任何错误，请服务器设置为在目录服务还原模式下启动调查的起点。  
  
## <a name="BKMK_VDCSafeRestore"></a>虚拟化保护  
与不同的虚拟化的域控制器复制，Windows Server 2012 虚拟化保护有没有配置步骤。 该功能适用无需参与，只要了解一些简单的条件：  
  
-   虚拟机监控程序支持 VM 代 ID  
  
-   没有可还原的域控制器可以将从更改非授权复制无效的合作伙伴域控制器。  
  
### <a name="validate-the-hypervisor"></a>验证虚拟机监控程序  
请确保源域控制器上受支持的虚拟机监控程序运行通过查看供应商文档。 虚拟化的域控制器虚拟机监控程序独立于，并不需要 HYPER-V。  
  
查看以前[平台要求](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_PlatformReqs)已知 VM 代 ID 支持的部分。  
  
如果要从源虚拟机监控程序到不同的目标虚拟机监控程序迁移虚拟机的功能，虚拟化保护可能或不可能在下表中所述触发根据管理程序是否支持 VM 代 ID。  
  
|源虚拟机监控程序|目标虚拟机监控程序|结果|  
|---------------------|---------------------|----------|  
|支持 VM 代 ID|不支持 VM 代 ID|不触发保护 （如果存在 DCCloneConfigFile.xml，直流将启动到 DSRM）|  
|不支持 VM 代 ID|支持 VM 代 ID|触发保护|  
|支持 VM 代 ID|支持 VM 代 ID|因为 VM 定义未更改，这意味着 VM 代 ID 保持不变，因此不触发保护|  
  
### <a name="validate-the-replication-topology"></a>验证复制拓扑  
虚拟化保护初始化非权威的入站的复制增量 Active Directory 复制以及非授权重新 SYSVOL 的所有内容同步。 这将确保域控制器从快照返回的全部功能，最终与环境其余部分保持一致。  
  
使用此新功能有多个要求和限制：  
  
-   还原的域控制器必须能够联系可写直流  
  
-   在某个域中的所有域控制器必须不能同时还原  
  
-   来自还原的域控制器尚未复制站由于拍摄快照的任何更改都将丢失永远停止营业  
  
尽管疑难解答部分涵盖这些方案，以下详细信息确保你未创建一个拓扑可能会导致出现问题。  
  
#### <a name="writable-domain-controller-availability"></a>可写的域控制器可用性  
如果恢复，域控制器必须具有连接到可写的域控制器;仅阅读域控制器无法发送更新增量。 拓扑是可能已，这为可写的域控制器正确始终需要可写的合作伙伴。 但是，如果所有可写的域控制器同时还原，其中任何一个可以找到有效的源。 也是如此如果可写的域控制器脱机维护或者不可通过该网络。  
  
#### <a name="simultaneous-restore"></a>同时还原  
不要同时还原单个域中的所有域控制器。 如果所有快照都还原一次性、 Active Directory 复制的工作方式正常，但 SYSVOL 复制将暂停运行。 还原的体系结构 FRS 和 DFSR 需要为非授权同步模式设置其副本实例。 如果所有域控制器都还原次，每个域控制器自己标记非权威的 SYSVOL，它们都然后尝试同步组策略和脚本从权威的合作伙伴;此时，但所有合作伙伴都还有非授权。  
  
> [!IMPORTANT]  
> 如果所有域控制器同时都还原，使用以下文章设置为授权，一个域控制器-通常 PDC 模拟器-，，以便其他域控制器可以返回到正常操作：  
>   
> [使用 BurFlags 注册表项重新初始化文件复制服务副本设置](https://support.microsoft.com/kb/290762)  
>   
> [如何为 （如"D4 /d2"的 FRS) 的 DFSR 复制 SYSVOL 强制权威和非授权同步](https://support.microsoft.com/kb/2218556)  
  
> [!WARNING]  
> 不要同一个虚拟机监控程序主机上的森林或域中运行的所有域控制器。 引入单点 cripples 广告 DS、 Exchange、 SQL 以及其他企业操作虚拟机监控程序离线每次的故障。 这是使用只有一个域控制器整个域或森林没有区别。 多个平台上的多个域控制器帮助提供冗余和出错的能力。  
  
#### <a name="post-snapshot-replication"></a>快照后复制  
不要还原所有本地原始以来所做更改复制快照创建站之前的快照。 原始的任何更改都将丢失永远停止营业如果其他域控制器未已经收到通过复制。  
  
使用 Repadmin.exe 显示域控制器和其合作伙伴之间的任何未复制站更改：  
  
1.  姓名和与 DSA 对象 Guid 返回 DC 合作伙伴：  
  
    ```  
    Repadmin.exe /showrepl <DC Name of the partner> /repsto  
    ```  
  
2.  返回到要还原的域控制器的合作伙伴域控制器待处理的入站的复制：  
  
    ```  
    Repadmin.exe /showchanges < Name of partner DC><DSA Object GUID of the domain controller being restored><naming context to compare>  
    ```  
  
或者，只会看到未复制变化数：  
  
```  
Repadmin.exe /showchanges <Name of partner DC><DSA Object GUID of the domain controller being restored><naming context to compare> /statistics  
```  
  
例如 (输出修改可便于阅读和重要的条目***斜体***)，你在此处查看 DC4 复制合作：  
  
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
  
现在，你知道正在 DC2 与 DC3 复制。 然后你会显示列表中更改 DC2 指出它仍不具有 DC4，从和查看新的一组：  
  
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
  
你还会测试的合作伙伴以确保它已无法已经复制。  
  
或者，如果你并不关心的对象有没有复制和仅太任何物体已出色，可以使用**/statistics**选项：  
  
```  
C:\>repadmin /showchanges dc2.corp.contoso.com 5d083398-4bd3-48a4-a80d-fb2ebafb984f dc=corp,dc=contoso,dc=com /statistics  
  
***********************************************  
********* Grand total *************************  
Packets:              1  
Objects:              1Object Additions:     1Object Modifications: 0Object Deletions:     0Object Moves:         0Attributes:           12Values:               13  
```  
  
> [!IMPORTANT]  
> 如果你看到任何失败或出色复制，测试所有可写的合作伙伴。 只要聚合至少一个它通常可以安全地还原快照，是因为传递复制最终调节其他服务器。  
>   
> 请务必注意复制显示 /showchanges 中的任何错误，不要继续直到修复这些问题。  
  
### <a name="windows-powershell-snapshot-cmdlets"></a>Windows 快照 PowerShell Cmdlet  
以下 Windows PowerShell HYPER-V 模块 cmdlet 提供在 Windows Server 2012 的快照功能：  
  
```  
Checkpoint-VM  
Export-VMSnapshot  
Get-VMSnapshot  
Remove-VMSnapshot  
Rename-VMSnapshot  
Restore-VMSnapshot  
```  
  


