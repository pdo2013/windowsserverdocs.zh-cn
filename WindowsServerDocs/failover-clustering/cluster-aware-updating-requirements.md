---
ms.assetid: 75cc1d24-fa2f-45bd-8f3b-1bbd4a1aead0
title: "群集的更新要求和最佳做法"
ms.prod: windows-server-threshold
ms.topic: article
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 4/28/2017
description: "使用群集的更新在运行 Windows Server 群集上安装更新的要求。"
ms.openlocfilehash: 8517466d58345077af446c1b2c1e2aeb3b17aaa1
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="cluster-aware-updating-requirements-and-best-practices"></a>群集的更新要求和最佳做法

>适用于：Windows Server（半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本部分介绍要求和依赖项所需使用[群集的更新](cluster-aware-updating.md)(CAU)，以适用于运行 Windows Server 故障转移群集的更新。
  
> [!NOTE]  
> 你可能需要独立验证群集环境已准备好将更新应用，如果你在以外的其他 plug\ 中使用**Microsoft.WindowsUpdatePlugin**。 如果你使用的 non\ Microsoft plug\ 中，请联系发布者的详细信息。 有关 plug\ 接的详细信息，请参阅[Plug\ 接的工作原理](cluster-aware-updating-plug-ins.md)。   
  
## <a name="BKMK_REQ_CLUS"></a>安装故障转移群集的功能和故障转移群集工具  
CAU 需要安装了故障转移群集的功能和故障转移群集工具。 故障转移群集工具包括 CAU 工具 \(clusterawareupdating.dll\)、故障转移群集的 cmdlet 和 CAU 运营所需的其他组件。 步骤安装故障转移群集的功能，请参阅[安装故障转移群集功能和工具](http://go.microsoft.com/fwlink/p/?LinkId=253342)。  
  
是否 CAU 坐标更新为群集故障转移群集上角色依赖故障转移群集工具的确切安装要求 \（通过使用 self\ 更新 mode\）或远程计算机。 CAU 的 self\ 更新模式此外通过 CAU 工具需要 CAU 聚集角色故障转移群集上的安装。    
  
下表总结了两种 CAU 更新模式 CAU 功能安装要求。  
  
|安装的组件|Self\ 更新模式|Remote\ 更新模式|  
|-----------------------|-----------------------|-------------------------|  
|故障转移群集功能|所需的所有群集节点|所需的所有群集节点|  
|故障转移群集工具|所需的所有群集节点|必需 remote\ 更新计算机上<br />-运行所需的所有群集节点[保存 CauDebugTrace](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/save-caudebugtrace) cmdlet|  
|CAU 聚集的角色|必填|不需要|  
  
## <a name="obtain-an-administrator-account"></a>获取管理员帐户  
以下管理员要求所需使用 CAU 功能。  
  
-   若要预览或使用 CAU 用户界面应用操作更新 \(UI\) 或群集的更新的 cmdlet，必须使用具有群集节点的本地管理员权限的域帐户。 如果该帐户上的每个节点没有足够的权限，提示你在窗口中群集的更新时执行这些操作，请提供必要的凭据。 若要使用[群集的更新](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating)cmdlet，你可以作为 cmdlet 参数提供必要的凭据。  
  
-   如果你使用 CAU remote\ 更新的方式使用没有本地管理员权限的群集节点的帐户登录时，你必须 CAU 工具以管理员身份运行通过更新处理协调器计算机上使用的本地管理员帐户或使用已帐户**模拟后身份验证的客户端**用户权限。 
  
-   若要运行 CAU 最佳做法分析工具，你必须使用具有管理员权限的群集节点和运行所用的计算机上的本地管理员权限的帐户[测试 CauSetup](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/test-causetup) cmdlet 或分析群集更新是否准备好使用群集的更新的窗口。 有关详细信息，请参阅[更新是否准备好测试群集](#BKMK_BPA)。  
  
## <a name="verify-the-cluster-configuration"></a>验证群集配置  
以下是常规要求故障转移群集来支持使用 CAU 更新。 远程管理节点上的其他配置要求中列出[配置进行远程管理节点](#BKMK_NODE_CONFIG)本主题后面。  
  
-   足够群集节点必须处于联机状态，以便具有仲裁的群集。  
  
-   在相同的域的 Active Directory 必须所有群集节点。  
  
-   必须先群集名称解决在使用 DNS 网络上。  
  
-   如果 CAU remote\ 更新模式中的使用，更新处理协调器计算机必须具有网络连接的故障转移群集节点，并且必须是同一个域的 Active Directory 故障转移群集。  
  
-   应运行所有群集节点上的群集服务。 默认情况下此服务所有群集节点上已安装并配置为自动启动。  
  
-   若要在 CAU 更新运行期间使用 PowerShell pre\ 更新或 post\ 更新脚本，确保上所有群集节点脚本，安装或，他们都可以访问所有节点，例如，在高度可用网络文件共享。 脚本将保存到网络文件共享，如果配置阅读权限的每个人的文件夹组。  
  
## <a name="BKMK_NODE_CONFIG"></a>配置进行远程管理节点  
若要使用群集的更新，必须为远程管理配置所有群集节点。 默认情况下，仅任务你必须执行配置节点远程管理是到[启用了防火墙规则允许自动重启](#BKMK_FW)。 

下表列出的完整远程管理要求，以防你环境不同的默认值。

除了安装要求将这些要求[安装故障转移群集的功能和故障转移群集工具](#BKMK_REQ_CLUS)和常规群集以前本主题中的各部分所述的要求。  
  
|要求|默认状态|Self\ 更新模式|Remote\ 更新模式|  
|---------------|---|-----------------------|-------------------------|  
|[启用了防火墙规则允许自动重新启动](#BKMK_FW)|已禁用|如果正在使用的防火墙被，必需的所有群集节点上|如果正在使用的防火墙被，必需的所有群集节点上|  
|[启用 Windows 管理检测](#BKMK_WMI)|启用|所需的所有群集节点|所需的所有群集节点|  
|[启用 Windows PowerShell 3.0 或 4.0 和 Windows PowerShell 远程处理](#BKMK_PS)|启用|所需的所有群集节点|所有群集节点都需要运行以下命令：<br /><br />-[保存 CauDebugTrace](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/save-caudebugtrace) cmdlet<br />-期间更新运行 PowerShell pre\ 更新和 post\ 更新脚本<br />-测试的群集更新是否准备好使用群集的更新窗口或[Test\ CauSetup](http://go.microsoft.com/fwlink/p/?LinkId=242384) Windows PowerShell cmdlet|  
|[安装.net 4.6 或 4.5](#BKMK_NET)|启用|所需的所有群集节点|所有群集节点都需要运行以下命令：<br /><br />-[保存 CauDebugTrace](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/save-caudebugtrace) cmdlet<br />-期间更新运行 PowerShell pre\ 更新和 post\ 更新脚本<br />-测试的群集更新是否准备好使用群集的更新窗口或[Test\ CauSetup](http://go.microsoft.com/fwlink/p/?LinkId=242384) Windows PowerShell cmdlet|  

### <a name="BKMK_FW"></a>启用了防火墙规则允许自动重新启动  
允许应用更新后的自动重新启动 \（如果更新的安装需要 restart\）、Windows 防火墙或 non\ Microsoft 防火墙群集节点上使用的是，如果必须防火墙规则启用允许以下交通每个节点上：  
  
-   协议：TCP  
  
-   方向：站  
  
-   计划：wininit.exe  
  
-   端口：RPC 动态端口  
  
-   个人资料：域  
  
如果 Windows 防火墙群集节点上使用，你可以通过启用**远程关机**每个群集节点 Windows 防火墙规则组。 当你使用的群集的更新的窗口才能将更新应用，并配置 self\ 更新的选项**远程关机**Windows 防火墙规则组自动启用每个群集节点。  
  
> [!NOTE]  
> **远程关机**时它将使用组策略设置为 Windows 防火墙配置相冲突，则不能启用 Windows 防火墙规则组。    
  
**远程关机**通过指定还启用了防火墙规则组**– EnableFirewallRules**参数运行以下 CAU cmdlet 时：[添加 CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/add-cauclusterrole)，[调用 CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-causcan)，并[SetCauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/set-cauclusterrole)。  
  
下面的示例 PowerShell 显示更多方法以启用群集节点上的自动重新启动。  
  
```PowerShell  
Set-NetFirewallRule -Group "@firewallapi.dll,-36751" -Profile Domain -Enabled true  
```  

### <a name="BKMK_WMI"></a>启用 Windows 管理 Instrumentation (WMI) 
必须进行远程管理使用 Windows 管理检测 \(WMI\) 配置所有群集节点。 这是默认启用。  
  
若要手动启用远程管理，请执行以下操作：  
  
1.  在服务控制台开始**Windows 远程管理**服务，并设置启动类型**自动**。  
  
2.  运行[组 WSManQuickConfig](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.wsman.management/set-wsmanquickconfig) cmdlet 或运行以下命令，从提升了权限的命令提示：  
  
    ```PowerShell  
    winrm quickconfig -q  
    ```  
  
是否上的群集节点 Windows 防火墙支持 WMI 远程的入站的防火墙规则**Windows 远程管理 \(HTTP\-In\)**必须为启用状态每个节点。  默认情况下，启用此规则。  
  
### <a name="BKMK_PS"></a>启用 Windows PowerShell 和 Windows PowerShell 远程处理  
若要启用 self\ 更新模式和 remote\ 更新模式中的某些 CAU 功能，必须安装并启用运行所有群集节点远程命令 PowerShell。 默认情况下，PowerShell 安装并启用远程处理。  
  
若要启用 PowerShell 远程处理，请使用以下方法之一：  
  
-   运行[启用 PSRemoting](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/enable-psremoting) cmdlet。  
  
-   Windows 远程管理 \(WinRM\) domain\ 级别组策略设置配置。  
  
有关启用 PowerShell 远程处理的详细信息，请参阅[about\_Remote\_Requirements](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/about/about_remote_requirements)。  
  
### <a name="BKMK_NET"></a>安装.net 4.6 或 4.5  
若要启用 self\ 更新模式和模式 remote\ 更新、.NET Framework 4.6 或.NET Framework 4.5（在 Windows Server 2012 R2) 中的某些 CAU 功能必须安装所有群集节点。 默认情况下，安装.NET Framework。  

若要安装.NET Framework 4.6（或 4.5）使用 PowerShell，如果尚未安装它，请使用以下命令：

```PowerShell
Install-WindowsFeature -Name NET-Framework-45-Core
```

## <a name="BKMK_BEST_PRAC"></a>最佳做法建议使用群集的更新 
  
### <a name="BKMK_BP_WUA"></a>有关 Microsoft 更新的应用建议

我们建议你开始使用 CAU 将默认使用的更新应用时**Microsoft.WindowsUpdatePlugin** plug\ 中群集上，阻止使用其他方法进行安装从 Microsoft 群集节点上的软件更新。  
  
> [!CAUTION]  
> 通过自动更新个别节点方法的组合 CAU \（在固定的时间计划 \) 可能导致意外的结果，包括服务和宕机中断。  
  
我们建议你遵循以下指南：  
  
-   为获得最佳效果，我们建议你禁用设置上的群集节点自动更新，例如，通过在控制面板，或配置为使用组策略的设置中的自动更新设置。  
  
    > [!CAUTION]  
    > 自动安装更新的群集节点上可能会影响安装的更新，方法是 CAU 与，并且会导致 CAU 失败。  
  
    如果需要以下自动更新设置是 CAU，与兼容的管理员可以控制的更新安装计时方式，因为：  
  
    -   若要之前下载更新通知，然后在安装之前通知设置  
  
    -   设置为自动下载更新，并且在安装之前通知  
  
    但是，如果自动更新下载更新，同时为 CAU 更新运行时，更新运行可能需要长时间才能完成。  
  
-   例如，Windows Server Update Services \(WSUS\) 应用自动更新不配置更新系统 \（在固定的时间计划 \) 群集节点向。  
  
-   应统一配置所有群集节点以使用相同的更新源，例如，WSUS 服务器、Windows 更新，或 Microsoft 更新。  
  
-   如果你使用配置管理系统应用到网络上计算机的软件更新，请从所有必需或自动更新排除群集节点。 配置管理系统的示例包括 Microsoft System Center Configuration Manager 2007 和 Microsoft 系统中心虚拟机经理 2008 年。  
  
-   如果内部软件 distribution 服务器 \ (例如，WSUS servers\) 用于包含和部署的更新，确保这些服务器正确识别群集节点批准的更新。  
  
#### <a name="BKMK_PROXY"></a>应用中分支机构方案 Microsoft 更新  
若要从 Microsoft 更新或 Windows 更新的 Microsoft 更新下载到某些分支机构方案中的群集节点，你可能需要在每个节点上配置为系统本地帐户的代理服务器设置。 例如，你可能需要你的分支机构群集访问 Microsoft 更新或 Windows 更新，方法是使用本地代理服务器下载更新时执行此操作。  
  
如有必要，每个节点指定本地代理服务器和配置本地地址例外上配置 WinHTTP 代理服务器设置 \（即，本地 addresses\ 忽略列表）。 若要执行此操作，你可以运行以下命令在每个群集节点从提升了权限的命令提示符下：  
  
```  
netsh winhttp set proxy <ProxyServerFQDN >:<port> "<local>"  
```  
  
其中 <*ProxyServerFQDN*> 是代理服务器完整的域名和 <*端口*> 是对其进行通信的端口（通常为端口 443）。  
  
例如，若要配置了 WinHTTP 指定代理服务器的系统本地帐户的代理服务器设置*MyProxy.CONTOSO.com*、端口 443 与本地地址例外，请键入以下命令：  
  
```  
netsh winhttp set proxy MyProxy.CONTOSO.com:443 "<local>"  
```  
  
### <a name="BKMK_BP_HF"></a>使用 Microsoft.HotfixPlugin 的建议  
  
-   我们建议你配置中的修补程序根文件夹和修补程序的配置文件，用于存储这些文件的计算机上本地仅管理员限制写入访问权限。 这可以帮助防止未经授权的用户，修复程序应用的情况下可能会破坏故障转移群集的功能这些文件篡改。  
  
-   为了帮助确保用于访问修补程序根文件夹服务器消息块 \(SMB\) 连接有关的数据完整性，你应配置 SMB 加密在 SMB 共享文件夹中，是否可以对其进行配置。 **Microsoft.HotfixPlugin**要求 SMB 登录或 SMB 加密配置以帮助确保 SMB 连接有关的数据完整性。 
  
    有关详细信息，请参阅[限制对的修补程序根文件夹和修补程序的配置文件的访问](cluster-aware-updating-plug-ins.md#BKMK_ACL)。
  
### <a name="additional-recommendations"></a>其他建议  
  
-   若要避免干扰 CAU 更新运行，可能同时计划，不要在计划的维护 windows 安排群集名称的对象和虚拟计算机对象密码更改。  
  
-   你应在未经授权的用户在网络共享文件夹以防止潜在篡改使用这些文件保存的 pre\ 更新和 post\ 更新脚本上设置相应的权限。  
  
-   在 self\ 更新模式中，虚拟计算机对象配置 CAU 必须在 Active Directory 中创建 \(VCO\) CAU 聚集角色。 CAU 可以此对象自动创建添加 CAU 聚集的角色，次如果故障转移群集具有足够的权限。 但是，某些组织中的安全策略，因为它可能需要预备中 Active Directory 对象。 若要执行此操作的过程，请参阅[预安排聚集角色帐户的步骤](http://go.microsoft.com/fwlink/p/?LinkId=237624)。  
  
-   保存和运行更新设置重新跨与故障转移群集类似更新需求 IT 部门，你可以创建更新运行配置文件。 此外，具体取决于更新模式中，你可以保存和管理更新运行配置文件在所有远程更新处理协调器计算机或故障转移群集访问文件共享。 有关详细信息，请参阅[高级选项和更新运行配置文件 CAU](cluster-aware-updating-options.md)。  
  
## <a name="BKMK_BPA"></a>更新准备情况的测试群集  
你可以运行测试是否故障转移群集 CAU 最佳实践分析 \(BPA\) 型号和网络环境满足的许多要求拥有 CAU 通过应用的软件更新。 许多测试检查是否准备好使用 plug\ 中的默认应用 Microsoft 更新的环境**Microsoft.WindowsUpdatePlugin**。  
  
> [!NOTE]  
> 你可能需要独立验证群集环境已准备好使用 plug\ 中以外的其他应用的软件更新**Microsoft.WindowsUpdatePlugin**。 如果你使用的 non\ Microsoft plug\ 中，如您硬件制造商提供的一项，请联系发布者的详细信息。  
  
你可以通过以下两种方式运行 BPA:  
  
1.  选择**更新准备情况的分析群集**中 CAU 主机。 BPA 完成准备好测试后，将显示一个测试报告。 如果群集节点上检测到问题，特定问题和出现问题的位置节点标识，以便你可以执行纠正操作。 测试可能需要几分钟才能完成。  
  
2.  运行[测试 CauSetup](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/test-causetup) cmdlet。 你可以安装该故障转移群集（故障转移群集工具的一部分）的 Windows PowerShell 模块的本地或远程计算机上运行 cmdlet。 你还可以在故障转移群集节点中运行 cmdlet。  
  
> [!NOTE]  
> -   你必须使用具有管理员权限的群集节点和运行所用的计算机上的本地管理员权限的帐户**Test\ CauSetup** cmdlet 或分析群集更新是否准备好使用群集的更新的窗口。 使用运行测试的群集的更新的窗口，你必须登录到计算机的必要的凭据。  
> -   测试假定用于预览和应用的软件更新 CAU 工具运行从同一台计算机和使用该相同的用户凭据用于测试群集更新准备好如。  
  
> [!IMPORTANT]  
> 我们强烈建议你测试更新准备好在以下情况下群集：  
>   
> -   之前的第一次 CAU 用于应用软件更新。  
> -   之后你添加到群集节点或执行群集需要运行验证群集向导中的其他硬件改动。  
> -   更改更新源，或更改更新设置或配置后 \(other than CAU\)，可能会影响更新节点上的应用。  
  
### <a name="tests-for-cluster-updating-readiness"></a>更新准备情况的群集的测试  
下表列出了群集更新准备好测试一些常见的问题，解决方法步骤。  
  
|测试|可能出现的问题以及的影响|解决方法步骤|  
|--------|-------------------------------|--------------------|  
|故障转移群集必须可用|不能解决故障转移群集名称，或者一个或多个群集节点不能访问。 BPA 不能运行了群集准备好测试。|-检查拼写群集运行 BPA 期间指定的名称。<br />-确保所有群集节点联机且正在运行。<br />-查看验证配置向导可以成功运行的故障转移群集上。|  
|必须进行远程管理通过 WMI 启用故障转移群集节点|一个或多个故障转移群集节点未通过 Windows 管理检测 \(WMI\) 启用进行远程管理。 如果未进行远程管理配置节点，CAU 无法更新群集节点。|确保启用远程管理通过 WMI 的所有故障转移群集节点。 有关详细信息，请参阅[配置进行远程管理节点](#BKMK_NODE_CONFIG)本主题中。|  
|每个故障转移群集节点启用 PowerShell 远程应启用|PowerShell 未安装，或者远程上一个或多个故障转移群集节点未启用。 CAU 无法配置为 self\ 更新模式或使用某些功能 remote\ 更新的方式。|确保 PowerShell 安装了所有群集节点，用于远程处理已启用。<br /><br />有关详细信息，请参阅[配置进行远程管理节点](#BKMK_NODE_CONFIG)本主题中。|  
|故障转移群集版本|一个或多个故障转移群集节点不运行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012。 CAU 无法更新故障转移群集。|验证过程运行 BPA 中指定的故障转移群集搭载 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012。<br /><br />有关详细信息，请参阅[验证群集配置](#BKMK_REQ_CLUS)本主题中。|  
|所有故障转移群集节点上必须安装所需的版本的.NET Framework 和 Windows PowerShell|.NET framework 4.6、4.5 或 Windows PowerShell 未安装一个或多个群集节点。 某些 CAU 功能可能无法工作。|如果需要，请确保所有的群集节点上安装了.NET Framework 4.6 或 4.5 和 Windows PowerShell。<br /><br />有关详细信息，请参阅[配置进行远程管理节点](#BKMK_NODE_CONFIG)本主题中。|  
|应运行所有群集节点上的群集服务|上一个或多个节点未运行的群集服务。 CAU 无法更新故障转移群集。|-确保群集服务 \(clussvc\) 开始群集，在所有节点上，并且配置为自动启动。<br />-查看验证配置向导可以成功运行的故障转移群集上。<br /><br />有关详细信息，请参阅[验证群集配置](#BKMK_REQ_CLUS)本主题中。|  
|自动更新不能配置为自动安装更新，在任何故障转移群集节点|在至少一个故障转移群集节点，将自动更新配置为自动安装该节点上的 Microsoft 更新。 与其他更新方法组合 CAU 可能会导致意外停止运行或无法预测结果。|如果 Windows 更新功能一个或多个群集节点上配置为自动更新，请确保自动更新未配置为自动安装更新。<br /><br />有关详细信息，请参阅[对 Microsoft 更新的应用建议](#BKMK_BP_WUA)。|  
|故障转移群集节点应使用相同的更新源|一个或多个故障转移群集节点配置更新源用于从其他节点不同的 Microsoft 更新。 更新可能不会统一应用上的群集节点 CAU 通过。|确保每群集节点配置为使用相同的更新源，例如，WSUS 服务器、Windows 更新，或 Microsoft 更新。<br /><br />有关详细信息，请参阅[对 Microsoft 更新的应用建议](#BKMK_BP_WUA)。|  
|允许远程关闭防火墙规则应每个在故障转移群集节点上启用|一个或多个故障转移群集节点不具有防火墙规则启用允许远程关机，也可能使用组策略设置会阻止此规则启用。 更新运行的应用更新，需要自动重启节点可能未正确完成。|Windows 防火墙或 non\ Microsoft 防火墙群集节点上使用的是，如果配置允许远程关闭防火墙规则。<br /><br />有关详细信息，请参阅[启用了防火墙规则允许自动重启](#BKMK_FW)本主题中。|  
|每个故障转移群集节点的代理服务器设置应到本地代理服务器设置|一个或多个故障转移群集节点有错误的代理服务器配置。<br /><br />如果本地代理服务器正在使用中，每个节点上的代理服务器设置必须群集访问 Microsoft 更新或 Windows 更新的正确配置。|确保它需要在每个群集节点 WinHTTP 代理服务器设置到本地代理服务器设置。 如果您的环境中使用不代理服务器，可以忽略此警告。<br /><br />有关详细信息，请参阅[应用更新中分支机构方案](#BKMK_PROXY)本主题中。|  
|若要启用 self\ 更新模式故障转移群集上应安装 CAU 聚集的角色|在此故障转移群集上未安装 CAU 聚集的角色。 此角色，才能进行群集 self\ 更新。|若要使用 self\ 更新型 CAU，添加 CAU 聚集的角色故障转移群集上通过以下方法之一：<br /><br />-运行[添加 CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/add-cauclusterrole) PowerShell cmdlet。<br />-选择**配置群集 self\ 更新选项**操作更新群集的窗口中。|  
|CAU 聚集的角色应启用故障转移群集上，若要启用 self\ 更新模式|CAU 聚集的角色处于禁用状态。 例如，CAU 聚集的角色未安装，或者使用它已禁用[Disable\ CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/disable-cauclusterrole) PowerShell cmdlet。 此角色，才能进行群集 self\ 更新。|若要使用 CAU 在 self\ 更新模式下，启用此故障转移群集上的 CAU 聚集的角色通过以下方法之一：<br /><br />-运行[启用 CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/enable-cauclusterrole) PowerShell cmdlet。<br />-选择**配置群集 self\ 更新选项**操作更新群集的窗口中。|  
|在适用于 self\ 更新模式的 plug\ 配置的 CAU 必须注册上所有故障转移群集节点|上一个或多个此故障转移群集节点 CAU 聚集的角色无法访问 CAU plug\ 的模块，配置在 self\ 更新的选项。 Self\ 更新运行可能失败。|-确保 plug\ 在配置的 CAU 上安装了所有群集节点按照以下产品提供 CAU plug\ 在安装过程。<br />-运行[Register\ CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin) PowerShell cmdlet 注册 plug\ 的所需的群集节点。|  
|所有故障转移群集节点应有的同一组注册 CAU plug\ 加载项|如果 plug\-在配置为在运行更新中使用更改为另一个不适用于所有群集节点，可能无法 self\ 更新运行。|-确保 plug\ 在配置的 CAU 上安装了所有群集节点按照以下产品提供 CAU plug\ 在安装过程。<br />-运行[Register\ CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin) PowerShell cmdlet 注册 plug\ 的所需的群集节点。|  
|必须有效的配置更新运行选项|Self\ 更新计划和配置为此故障转移群集的更新运行选项并不完整或无效。 Self\ 更新运行可能失败。|配置有效 self\ 更新计划和运行更新选项的设置。 例如，你可以使用[Set\ CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/set-cauclusterrole) PowerShell cmdlet 配置 CAU 群集且的角色。|  
|必须 CAU 聚集角色的拥有者至少两个故障转移群集节点。|更新运行启动 self\ 更新型将失败，因为 CAU 聚集的角色却没有将移动到一个可能的所有者节点。|使用故障转移群集工具确保所有群集节点上都配置了为可能的所有者的 CAU 聚集角色。 这是默认配置。||  
|所有故障转移群集节点必须是能够访问 Windows PowerShell 脚本|并非所有可能的所有者节点 CAU 聚集角色的可以访问已配置的 Windows PowerShell pre\ 更新和 post\ 更新脚本。 将失败 self\ 更新运行。|确保 CAU 聚集角色的所有可能的所有者节点有权访问已配置 PowerShell pre\ 的更新和 post\ 更新脚本。|  
|所有故障转移群集节点应使用相同的 Windows PowerShell 脚本|并非所有可能的所有者节点 CAU 聚集角色的使用同一指定的 Windows PowerShell pre\ 更新和 post\ 更新脚本的副本。 Self\ 更新运行可能无法或显示异常的问题。|确保所有可能的所有者节点 CAU 聚集角色的使用相同的 PowerShell pre\ 更新和 post\ 更新脚本。|  
|有关更新运行指定 WarnAfter 设置应小于 StopAfter 设置|指定的 CAU 更新运行超时值使警告超时无效。 可生成一个警告事件日志之前，可能会取消更新运行。|在更新运行选项中，将配置**WarnAfter**选项该键值小于**StopAfter**值的选项。|  
  
## <a name="see-also"></a>请参阅  
  
-   [群集的更新概述](cluster-aware-updating.md)
  
