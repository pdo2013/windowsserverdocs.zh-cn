---
ms.assetid: 75cc1d24-fa2f-45bd-8f3b-1bbd4a1aead0
title: 群集感知更新要求和最佳做法
ms.prod: windows-server-threshold
ms.topic: article
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 08/06/2018
description: 使用群集感知更新在运行 Windows Server 的群集上安装更新的要求。
ms.openlocfilehash: 379c3caa39b09e8a912150f2423190e143991c05
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819728"
---
# <a name="cluster-aware-updating-requirements-and-best-practices"></a>群集感知更新要求和最佳做法

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

本部分介绍的要求和使用所需的依赖项[群集感知更新](cluster-aware-updating.md)(CAU) 将更新应用于运行 Windows Server 故障转移群集。
  
> [!NOTE]  
> 可能需要独立验证群集环境是否已准备好应用更新，如果使用即插即用\-比其他**Microsoft.WindowsUpdatePlugin**。 如果使用非\-Microsoft 即插即用\-中，有关详细信息联系发布者。 有关插件的详细信息\-项，请参阅[如何插入\-项工作](cluster-aware-updating-plug-ins.md)。   
  
## <a name="BKMK_REQ_CLUS"></a>安装故障转移群集功能和故障转移群集工具  
CAU 需要安装故障转移群集功能和故障转移群集工具。 故障转移群集工具包括 CAU 工具\(clusterawareupdating.dll\)，故障转移群集 cmdlet 和 CAU 操作所需的其他组件。 有关安装故障转移群集功能的步骤，请参阅 [安装故障转移群集功能和工具](create-failover-cluster.md#install-the-failover-clustering-feature)。  
  
故障转移群集工具的准确安装要求取决于是否 CAU 协调更新作为故障转移群集上群集角色\(通过使用自我\-更新模式\)或从远程计算机。 自\-此外更新模式的 CAU 需要安装 CAU 群集角色在故障转移群集上通过使用 CAU 工具。    
  
下表总结了针对两种 CAU 更新模式的 CAU 功能安装要求。  
  
|已安装的组件|自助\-更新模式|远程\-更新模式|  
|-----------------------|-----------------------|-------------------------|  
|故障转移群集功能|在所有群集节点上必需|在所有群集节点上必需|  
|故障转移群集工具|在所有群集节点上必需|所需远程\-更新的计算机<br />所需的所有群集节点上运行[Save-caudebugtrace](https://docs.microsoft.com/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps) cmdlet|  
|CAU 群集角色|必需|不必需|  
  
## <a name="obtain-an-administrator-account"></a>获取管理员帐户  
以下管理员要求是使用 CAU 功能所必需的。  
  
-   若要预览或应用更新操作通过使用 CAU 用户界面\(UI\)或的群集感知更新 cmdlet，必须使用所有群集节点具有本地管理员权利和权限的域帐户。 如果帐户没有足够的权限，每个节点上，系统会提示在群集感知更新窗口中执行这些操作时提供所需的凭据。 若要使用[群集感知更新](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps)cmdlet，你可以提供所需的凭据作为 cmdlet 参数。  
  
-   如果在远程使用 CAU\-更新模式时不会在群集节点上具有本地管理员权利和权限的帐户登录，您必须运行 CAU 工具以管理员身份通过在使用本地管理员帐户更新协调器计算机，或通过使用具有的帐户**身份验证后模拟客户端**用户权限。 
  
-   若要运行 CAU 最佳实践分析工具，必须使用具有管理权限在群集节点上的以及用来运行的计算机上的本地管理权限的帐户[Test-causetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps) cmdlet 或分析群集更新准备情况使用群集感知更新窗口。 有关详细信息，请参阅 [测试群集更新准备情况](#BKMK_BPA)。  
  
## <a name="verify-the-cluster-configuration"></a>验证群集配置  
以下是针对故障转移群集使用 CAU 以支持更新的一般需求。 针对节点上远程管理的其他配置要求在本主题后面部分的 [为远程管理配置节点](#BKMK_NODE_CONFIG) 中列出。  
  
-   足够的群集节点必须处于联机状态，以便群集具有仲裁资格。  
  
-   所有群集节点都必须位于同一个 Active Directory 域中。  
  
-   必须使用 DNS 在网络上解析群集名称。  
  
-   如果在远程使用 CAU\-更新模式，更新协调器计算机必须具有网络连接到故障转移群集节点，并且它必须为故障转移群集相同的 Active Directory 域中。  
  
-   群集服务应在所有群集节点上运行。 默认情况下，此服务将在所有群集节点上安装并配置为自动启动。  
  
-   若要使用 PowerShell 预\-更新或发布\-CAU 更新运行期间更新脚本，请确保脚本将安装在所有群集节点上或者它们是否可以访问所有节点，例如，在高度可用的网络文件共享。 如果将脚本保存到网络文件共享，请为 Everyone 组配置对该文件夹的读取权限。  
  
## <a name="BKMK_NODE_CONFIG"></a>配置用于远程管理的节点  
若要使用群集感知更新，必须为远程管理配置群集的所有节点。 默认情况下，唯一的任务时所要执行到为远程管理配置节点[启用防火墙规则以允许自动重新启动](#BKMK_FW)。 

下表列出对完整的远程管理的要求，如果您的环境有何区别默认值。

这些要求是对[安装故障转移群集功能和故障转移群集工具](#BKMK_REQ_CLUS)安装要求和在本主题前面部分中所述的常规群集要求的补充。  
  
|要求|默认状态|自助\-更新模式|远程\-更新模式|  
|---------------|---|-----------------------|-------------------------|  
|[启用防火墙规则以允许自动重新启动](#BKMK_FW)|Disabled|如果防火墙正在使用中，则在所有群集节点上都是必需的|如果防火墙正在使用中，则在所有群集节点上都是必需的|  
|[启用 Windows 管理规范](#BKMK_WMI)|Enabled|在所有群集节点上必需|在所有群集节点上必需|  
|[启用 Windows PowerShell 3.0 或 4.0 和 Windows PowerShell 远程处理](#BKMK_PS)|Enabled|在所有群集节点上必需|若要运行以下组件，则在所有群集节点上必需：<br /><br />- [Save-caudebugtrace](https://docs.microsoft.com/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps) cmdlet<br />-PowerShell pre\-更新并发布\-更新运行期间更新脚本<br />-测试群集更新准备情况使用群集感知更新窗口或[测试\-CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps) Windows PowerShell cmdlet|  
|[安装.NET Framework 4.6 或 4.5](#BKMK_NET)|Enabled|在所有群集节点上必需|若要运行以下组件，则在所有群集节点上必需：<br /><br />- [Save-caudebugtrace](https://docs.microsoft.com/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps) cmdlet<br />-PowerShell pre\-更新并发布\-更新运行期间更新脚本<br />-测试群集更新准备情况使用群集感知更新窗口或[测试\-CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps) Windows PowerShell cmdlet|  

### <a name="BKMK_FW"></a>启用防火墙规则以允许自动重新启动  
若要允许应用更新后自动重新启动\(如果更新的安装要求重启\)，如果 Windows 防火墙或非\-Microsoft 防火墙在群集节点上的使用，必须在上启用防火墙规则每个节点都允许以下流量：  
  
-   协议：TCP  
  
-   方向：入站  
  
-   程序：wininit.exe  
  
-   端口：RPC 动态端口  
  
-   配置文件：域  
  
如果在群集节点上使用 Windows 防火墙，则可以通过在每个群集节点上启用“远程关机”  Windows 防火墙规则组来执行此操作。 当使用群集感知更新窗口，以应用更新，并配置自我\-更新选项，**远程关机**每个群集节点上自动启用 Windows 防火墙规则组。  
  
> [!NOTE]  
> **Remote Shutdown** Windows 防火墙规则组在与为 Windows 防火墙配置的组策略设置发生冲突时无法启用。    
  
**远程关机**防火墙规则组也启用通过指定 **– EnableFirewallRules**参数运行以下 CAU cmdlet 时：Add-CauClusterRole、[Invoke-CauRun](https://docs.microsoft.com/powershell/module/clusterawareupdating/Invoke-CauRun?view=win10-ps) 和 [SetCauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Set-CauClusterRole?view=win10-ps)。  
  
以下 PowerShell 示例显示了其他方法来实现自动重新启动群集节点上。  
  
```PowerShell  
Set-NetFirewallRule -Group "@firewallapi.dll,-36751" -Profile Domain -Enabled true  
```  

### <a name="BKMK_WMI"></a>启用 Windows 管理规范 (WMI) 
必须将所有群集节点都配置为远程管理使用 Windows Management Instrumentation \(WMI\)。 默认情况下该协议处于启用状态。  
  
若要手动启用远程管理，请执行以下操作：  
  
1.  在服务控制台中，启动“Windows 远程管理”  服务并将启动类型设置为“自动” 。  
  
2.  运行[Set-wsmanquickconfig](https://docs.microsoft.com/powershell/module/Microsoft.WsMan.Management/Set-WSManQuickConfig?view=powershell-6) cmdlet，或者运行以下命令从提升的命令提示符：  
  
    ```PowerShell  
    winrm quickconfig -q  
    ```  
  
若要支持 WMI 远程处理，如果 Windows 防火墙正在使用群集节点上，为入站的防火墙规则**Windows 远程管理\(HTTP\-中\)** 必须在每个节点上启用。  默认情况下，启用此规则。  
  
### <a name="BKMK_PS"></a>启用 Windows PowerShell 和 Windows PowerShell 远程处理  
若要启用自助\-更新模式和远程中的某些 CAU 功能\-更新模式下，PowerShell 必须安装并启用所有群集节点上运行远程命令。 默认情况下，PowerShell 是安装和启用远程处理。  
  
若要启用 PowerShell 远程处理，请使用以下方法之一：  
  
-   运行[Enable-psremoting](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Enable-PSRemoting) cmdlet。  
  
-   配置域\-级组策略设置为 Windows 远程管理\(WinRM\)。  
  
有关启用 PowerShell 远程处理的详细信息，请参阅[关于远程要求](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_remote_requirements?view=powershell-6)。  
  
### <a name="BKMK_NET"></a>安装.NET Framework 4.6 或 4.5  
若要启用自助\-更新模式和远程中的某些 CAU 功能\-更新模式、.NET Framework 4.6 或.NET Framework 4.5 （在 Windows Server 2012 R2) 上必须安装在所有群集节点上。 默认情况下，安装.NET Framework。  

若要安装.NET Framework 4.6 （或 4.5），它使用 PowerShell，如果尚未安装，请使用以下命令：

```PowerShell
Install-WindowsFeature -Name NET-Framework-45-Core
```

## <a name="BKMK_BEST_PRAC"></a>使用群集感知更新的最佳实践建议 
  
### <a name="BKMK_BP_WUA"></a>有关应用 Microsoft 更新的建议

我们建议，当你开始使用 CAU 来应用更新的默认值**Microsoft.WindowsUpdatePlugin**插入\-中的群集上，你停止使用其他方法在群集上安装来自 Microsoft 的软件更新节点。  
  
> [!CAUTION]  
> 将 CAU 与自动更新各个节点的方法相结合\(按固定的时间计划\)可能会导致不可预知的结果，包括服务和计划外停机时间中的中断。  
  
建议遵循以下原则：  
  
-   为了获得最佳结果，我们建议你针对自动更新禁用群集节点上的设置。例如，通过控制面板中的自动更新设置，或在使用组策略进行配置的设置中禁用。  
  
    > [!CAUTION]  
    > 在群集节点上的自动更新安装可能会干扰通过 CAU 进行的更新安装，并且可能会导致 CAU 失败。  
  
    如果需要，以下自动更新设置与 CAU 兼容，因为管理员可以控制更新安装的计时：  
  
    -   在下载更新之前以及在安装之前用于通知的设置  
  
    -   用于自动下载更新以及在安装之前用于通知的设置  
  
    但是，如果自动更新下载更新的时间与 CAU 更新运行的时间相同，则更新运行可能需要更长时间才能完成。  
  
-   未配置更新系统，如 Windows Server Update Services \(WSUS\)以自动应用更新\(按固定的时间计划\)到群集节点。  
  
-   所有群集节点应统一地配置为使用相同的更新源，例如，WSUS 服务器、Windows 更新或 Microsoft 更新。  
  
-   如果使用配置管理系统对网络上的计算机应用软件更新，请从所有必需的或自动的更新中排除群集节点。 配置管理系统的示例包括 Microsoft System Center Configuration Manager 2007 和 Microsoft System Center Virtual Machine Manager 2008。  
  
-   如果内部软件分发服务器\(例如，WSUS 服务器\)用于包含和部署更新，请确保这些服务器正确标识群集节点的已批准的更新。  
  
#### <a name="BKMK_PROXY"></a>应用在分支机构方案中的 Microsoft 更新  
若要从 Microsoft 更新或 Windows 更新将 Microsoft 更新下载到某些分支机构方案中的群集节点，你可能需要配置每个节点上的本地系统帐户的代理设置。 例如，如果分支机构群集访问 Microsoft 更新或 Windows 更新以通过使用本地代理服务器下载更新，则可能需要执行此操作。  
  
如有必要，可以指定本地代理服务器和配置本地地址例外每个节点上配置 WinHTTP 代理设置\(也就是说，对于本地地址跳过列表\)。 若要执行此操作，你可以在每个群集节点上从提升的命令提示符运行以下命令：  
  
```  
netsh winhttp set proxy <ProxyServerFQDN >:<port> "<local>"  
```  
  
其中 <*ProxyServerFQDN*> 是代理服务器的完全限定的域名和 <*端口*> 是要对其进行通信的端口 （通常为端口 443）。  
  
例如，若要配置的指定代理服务器的本地系统帐户的 WinHTTP 代理设置*MyProxy.CONTOSO.com*、 使用端口 443 和本地地址例外，键入以下命令：  
  
```  
netsh winhttp set proxy MyProxy.CONTOSO.com:443 "<local>"  
```  
  
### <a name="BKMK_BP_HF"></a>使用 Microsoft.HotfixPlugin 的建议  
  
-   我们建议你将修补程序根文件夹和修补程序配置文件的权限配置为只限制用于存储这些文件的计算机上本地管理员的写入访问权限。 当应用修补程序时，这有助于防止未经授权的用户篡改这些文件，这些用户可能损害故障转移群集的功能。  
  
-   若要帮助确保服务器消息块的数据完整性\(SMB\)用于访问修补程序根文件夹的连接，你应配置 SMB 加密在 SMB 共享文件夹中，它是否可以对其进行配置。 **Microsoft.HotfixPlugin** 要求 SMB 签名或 SMB 加密配置为帮助确保 SMB 连接的数据完整性。 
  
    有关详细信息，请参阅[限制对修补程序根文件夹和修补程序配置文件访问](cluster-aware-updating-plug-ins.md#BKMK_ACL)。
  
### <a name="additional-recommendations"></a>更多建议  
  
-   为了避免干扰可能在同一时间计划的 CAU 更新运行，不要在计划的维护时段安排群集名称对象和虚拟计算机对象的密码更改。  
  
-   应上预设置适当的权限\-更新并发布\-更新脚本保存在网络上共享文件夹，以防止可能篡改这些文件未经授权的用户。  
  
-   若要在自助配置 CAU\-更新模式下，虚拟计算机对象\(VCO\)为 CAU 群集的角色必须是 Active Directory 中创建。 如果故障转移群集具有足够的权限，则 CAU 可以在添加 CAU 群集角色时自动创建此对象。 但是，由于某些组织中的安全策略，可能需要在 Active Directory 中预留对象。 有关执行此操作的过程，请参阅 [预安排群集角色的帐户的步骤](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731002\(v=ws.10\)#steps-for-prestaging-the-cluster-name-account)。  
  
-   若要在 IT 组织中根据相似更新需求跨故障转移群集保存和重复使用更新运行设置，你可以创建更新运行配置文件。 此外，根据更新模式，你可以在所有远程更新协调器计算机或故障转移群集均可访问的文件共享上保存和管理更新运行配置文件。 有关详细信息，请参阅[高级选项和更新运行配置文件适用于 CAU](cluster-aware-updating-options.md)。  
  
## <a name="BKMK_BPA"></a>测试群集更新准备情况  
可以运行 CAU 最佳做法分析器\(BPA\)要测试是否故障转移群集和网络环境满足由 CAU 应用软件更新的要求的许多模型。 很多测试检查环境应用 Microsoft 更新使用默认插件的准备情况\-在中， **Microsoft.WindowsUpdatePlugin**。  
  
> [!NOTE]  
> 可能需要独立验证群集环境是否已准备好应用软件更新使用即插即用\-比其他**Microsoft.WindowsUpdatePlugin**。 如果使用非\-Microsoft 即插即用\-中，例如硬件制造商提供的联系了解详细信息发布服务器。  
  
你可以采用以下两种方式运行 BPA：  
  
1.  在 CAU 控制台中选择“分析群集更新准备情况”  。 BPA 完成准备情况测试后，会显示测试报告。 如果在群集节点上检测到问题，在问题出现的位置标识其中特定的问题和节点，以便可以采取纠正措施。 测试可能需要几分钟才能完成。  
  
2.  运行[Test-causetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup) cmdlet。 您可以在其安装故障转移群集适用于 Windows PowerShell 模块 （故障转移群集工具的一部分） 的本地或远程计算机上运行 cmdlet。 你还可以在故障转移群集的节点上运行该 cmdlet。  
  
> [!NOTE]  
> -   必须使用具有管理权限在群集节点上的以及用来运行的计算机上的本地管理权限的帐户**测试\-CauSetup** cmdlet 或分析群集更新准备情况使用群集感知更新窗口。 若要运行的测试使用群集感知更新窗口，您必须登录到计算机所需的凭据。  
> -   测试假设用于预览和应用软件更新的 CAU 工具在同一台计算机上运行，并且使用与用于测试群集更新准备情况的凭据相同的用户凭据。  
  
> [!IMPORTANT]  
> 我们强烈建议你在以下情况中测试群集更新准备情况：  
>   
> -   在你第一次使用 CAU 应用软件更新前。  
> -   在你将节点添加到群集后，或者在需要运行“验证群集”向导的群集中执行其他硬件更改后。  
> -   更改更新源，或更改更新设置或配置后\(除 CAU 以外\)，可以影响节点上的更新的应用程序。  
  
### <a name="tests-for-cluster-updating-readiness"></a>群集更新准备情况的测试  
下表列出了群集更新准备情况测试、一些常见问题和解决步骤。  
  
|测试|可能的问题和影响|解决步骤|  
|--------|-------------------------------|--------------------|  
|故障转移群集必须可用|无法解析该故障转移群集名称，或者无法访问一个或多个群集节点。 BPA 无法运行群集准备情况测试。|-检查在 BPA 运行过程中指定的群集的名称的拼写。<br />-确保群集的所有节点都处于联机状态且正在运行。<br />-检查验证配置向导可以成功运行故障转移群集上。|  
|通过 WMI 的远程管理，必须启用故障转移群集节点|一个或多个故障转移群集节点未启用远程管理通过使用 Windows Management Instrumentation \(WMI\)。 如果未对远程管理配置节点，则 CAU 无法更新群集节点。|确保通过 WMI 的远程管理启用所有故障转移群集节点。 有关详细信息，请参阅本主题中的[配置用于远程管理的节点](#BKMK_NODE_CONFIG)。|  
|应在每个故障转移群集节点上启用 PowerShell 远程处理|PowerShell 未安装或未启用一个或多个故障转移群集节点上的远程处理。 不能为己方配置 CAU\-更新模式下或在远程使用某些功能\-更新模式。|请确保 PowerShell 所有群集节点上安装和启用远程处理。<br /><br />有关详细信息，请参阅本主题中的[配置用于远程管理的节点](#BKMK_NODE_CONFIG)。|  
|故障转移群集版本|故障转移群集中的一个或多个节点不运行 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012。 CAU 无法更新故障转移群集。|验证在 BPA 运行过程中指定的故障转移群集正在运行 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012。<br /><br />有关详细信息，请参阅本主题中的 [验证群集配置](#BKMK_REQ_CLUS) 。|  
|必须在所有故障转移群集节点上安装 .NET Framework 和 Windows PowerShell 的所需版本|在一个或多个群集节点上未安装.NET framework 4.6 中，4.5 或 Windows PowerShell。 某些 CAU 功能可能无法工作。|如有必要，请确保所有的群集节点上安装.NET Framework 4.6 或 4.5 和 Windows PowerShell。<br /><br />有关详细信息，请参阅本主题中的[配置用于远程管理的节点](#BKMK_NODE_CONFIG)。|  
|群集服务应在所有群集节点上运行|群集服务未在一个或多个节点上运行。 CAU 无法更新故障转移群集。|-确保群集服务\(clussvc\)在群集中的所有节点上启动且配置为自动启动。<br />-检查验证配置向导可以成功运行故障转移群集上。<br /><br />有关详细信息，请参阅本主题中的 [验证群集配置](#BKMK_REQ_CLUS) 。|  
|自动更新不能配置为在任何故障转移群集节点上自动安装更新|至少在一个故障转移群集节点上，自动更新配置为在该节点上自动安装 Microsoft 更新。 将 CAU 与其他更新方法结合起来使用可能会导致非计划停机时间或无法预料的结果。|如果 Windows 更新功能在一个或多个群集节点上配置为自动更新，请确保自动更新未配置为自动安装更新。<br /><br />有关详细信息，请参阅 [有关应用 Microsoft 更新的建议](#BKMK_BP_WUA)。|  
|故障转移群集节点应使用相同的更新源|一个或多个故障转移群集节点配置为针对 Microsoft 更新使用与其余节点不同的更新源。 可能不会通过 CAU 在群集节点上统一应用更新。|请确保将每个群集节点配置为使用相同的更新源，例如，WSUS 服务器、Windows 更新或 Microsoft 更新。<br /><br />有关详细信息，请参阅 [有关应用 Microsoft 更新的建议](#BKMK_BP_WUA)。|  
|允许应在故障转移群集中的每个节点上启用远程关机的防火墙规则|一个或多个故障转移群集节点不启用允许远程关机的防火墙规则，或者组策略设置阻止启用此规则。 应用需要自动重新启动节点的更新的更新运行可能未正确完成。|如果 Windows 防火墙或非\-Microsoft 防火墙正在使用群集节点上，配置允许远程关机的防火墙规则。<br /><br />有关详细信息，请参阅本主题中的[启用防火墙规则以允许自动重新启动](#BKMK_FW)。|  
|在每个故障转移群集节点上的代理服务器设置应设置为本地代理服务器|一个或多个故障转移群集节点具有不正确的代理服务器配置。<br /><br />如果本地代理服务器正在使用中，则每个节点上的代理服务器设置必须正确配置以便群集能够访问 Microsoft 更新或 Windows 更新。|如果需要，请确保将每个群集节点上的 WinHTTP 代理设置配置为本地代理服务器。 如果你的环境中没有使用代理服务器，则可以忽略此警告。<br /><br />有关详细信息，请参阅本主题中的[在分支机构方案中应用更新](#BKMK_PROXY)。|  
|若要启用自动故障转移群集上应安装 CAU 群集的角色\-更新模式|在此故障转移群集上未安装 CAU 群集角色。 此角色是必需的群集自\-更新。|若要使用 CAU 中自助\-更新模式，通过以下方式之一在故障转移群集上添加 CAU 群集的角色：<br /><br />-运行[Add-cauclusterrole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Add-CauClusterRole) PowerShell cmdlet。<br />-选择**配置群集自我\-更新选项**群集感知更新窗口中的操作。|  
|CAU 群集的角色应启用故障转移群集上，若要启用自动\-更新模式|CAU 群集角色处于禁用状态。 例如，未安装 CAU 群集的角色，或已禁用通过使用[禁用\-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Disable-CauClusterRole) PowerShell cmdlet。 此角色是必需的群集自\-更新。|若要使用 CAU 中自助\-更新模式，通过以下方式之一启用 CAU 群集的角色在此故障转移群集上：<br /><br />-运行[Enable-cauclusterrole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Enable-CauClusterRole) PowerShell cmdlet。<br />-选择**配置群集自我\-更新选项**群集感知更新窗口中的操作。|  
|已配置的 CAU 插件\-中的自助\-必须在所有故障转移群集节点上注册更新模式|此故障转移群集的一个或多个节点上的 CAU 群集的角色不能访问 CAU 插件\-自身中配置的模块中\-更新选项。 自\-更新运行可能会失败。|-确保已配置的 CAU 插件\-在安装在所有群集节点上通过遵循提供 CAU 插件的产品的安装过程\-中。<br />-运行[注册\-CauPlugin](https://docs.microsoft.com/powershell/module/clusterawareupdating/Register-CauPlugin) PowerShell cmdlet 以注册即插即用\-中必需的群集节点上。|  
|所有故障转移群集节点应具有一组相同的已注册 CAU 插件\-项|自\-更新运行可能会失败，如果即插即用\-中，配置要在更新运行中使用已更改为另一个在所有群集节点上不可用。|-确保已配置的 CAU 插件\-在安装在所有群集节点上通过遵循提供 CAU 插件的产品的安装过程\-中。<br />-运行[注册\-CauPlugin](https://docs.microsoft.com/powershell/module/clusterawareupdating/Register-CauPlugin) PowerShell cmdlet 以注册即插即用\-中必需的群集节点上。|  
|配置的更新运行选项必须是有效的|自\-更新计划和更新运行选项为此故障转移群集配置不完整或无效。 自\-更新运行可能会失败。|配置的有效自\-更新计划和更新运行选项集。 例如，可以使用[设置\-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Set-CauClusterRole) PowerShell cmdlet 配置 CAU 群集角色。|  
|至少两个故障转移群集节点都必须是 CAU 群集角色的所有者|在自启动的更新运行\-更新模式下将失败，因为 CAU 群集的角色不具有要移动到可能的所有者节点。|使用故障转移群集工具来确保所有群集节点都配置为 CAU 群集角色的可能所有者。 这是默认配置。||  
|所有故障转移群集节点都必须能够访问 Windows PowerShell 脚本|并非所有 CAU 群集角色的可能所有者节点可以都访问已配置的 Windows PowerShell pre\-更新并发布\-更新脚本。 自\-更新运行将失败。|确保所有 CAU 群集角色的可能所有者节点都有权访问已配置的 PowerShell pre\-更新并发布\-更新脚本。|  
|所有故障转移群集节点应使用相同的 Windows PowerShell 脚本|并非所有 CAU 群集角色的可能所有者节点都使用指定的 Windows PowerShell 之前的相同副本\-更新并发布\-更新脚本。 自\-更新运行可能会失败或显示意外的行为。|确保所有 CAU 群集角色的可能所有者节点都使用相同的 PowerShell pre\-更新并发布\-更新脚本。|  
|为更新运行指定的 WarnAfter 设置应小于 StopAfter 设置|指定的 CAU 更新运行超时值可以使警告超时无效。 在警告事件日志生成之前，可能会取消更新运行。|在更新运行选项中，配置小于“StopAfter”  选项值的“WarnAfter”  选项值。|  
  
## <a name="see-also"></a>请参阅  
  
-   [群集感知更新概述](cluster-aware-updating.md)