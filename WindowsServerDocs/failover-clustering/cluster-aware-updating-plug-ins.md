---
ms.assetid: d44d4430-41e2-483a-9685-92610cdef32a
title: 群集感知更新插件的工作原理
ms.topic: article
ms.prod: windows-server-threshold
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 04/28/2017
ms.technology: storage-failover-clustering
description: 如何使用插件来协调更新时使用群集感知更新 Windows Server 中的群集上安装更新。
ms.openlocfilehash: d09addb5e6787a8386d50570c0d27640646aa587
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854558"
---
# <a name="how-cluster-aware-updating-plug-ins-work"></a>群集感知更新插件的工作原理

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

[群集感知更新](cluster-aware-updating.md)(CAU) 使用插件来协调跨故障转移群集中节点的更新的安装。 本主题介绍如何使用内置\-中 CAU 插件\-接程序或其他即插即用\-你为 CAU 安装的项。

## <a name="BKMK_INSTALL"></a>安装即插即用\-中  
即插即用\-中而不是默认插入\-使用 CAU 安装的项\( **Microsoft.WindowsUpdatePlugin**并**Microsoft.HotfixPlugin** \)必须单独安装。 如果在自身中使用 CAU\-更新模式，即插即用\-中必须安装在所有群集节点上。 如果在远程使用 CAU\-更新模式，即插即用\-中必须安装在远程更新协调器计算机上。 即插即用\-安装每个节点上可能有其他安装要求。  
  
若要安装插件\-中，按照说明进行操作即插即用\-在发布服务器中。 若要手动注册插件\-使用 CAU，运行[Register-cauplugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin)每台计算机上的 cmdlet，即插即用\-安装中。  
  
## <a name="specify-a-plug-in-and-plug-in-arguments"></a>指定插件\-中并将插入\-in 自变量  
  
### <a name="specify-a-cau-plug-in"></a>指定 CAU 插件\-中

在 CAU UI 中，选择即插即用\-中从下拉\-可用插件列表中向下\-项时使用 CAU 来执行以下操作：  
  
-   将更新应用到群集  
  
-   群集的预览更新  
  
-   配置群集自我\-更新选项  
  
默认情况下，CAU 选择插件\-中**Microsoft.WindowsUpdatePlugin**。 但是，可以指定任何即插即用\-，安装并注册使用 CAU。

> [!TIP]  
> 在 CAU UI 中，仅可以指定单个即插即用\-中为 CAU 预览或应用更新运行期间更新。 通过使用 CAU PowerShell cmdlet，你可以指定一个或多个插件\-项。如果需要在群集上安装多个类型的更新，则指定多个插件通常更高效\-一个更新运行，而不是为每个插件使用单独更新运行中的项\-中。 例如，通常会发生更少的节点重新启动。

通过使用下表中列出的 CAU PowerShell cmdlet，您可以指定一个或多个插件\-接程序，为更新运行或通过扫描 **– CauPluginName**参数。 您可以指定多个插件\-中通过以逗号分隔的名称。 如果指定多个插件\-项，您还可以控制如何插头\-相互影响更新运行期间通过指定 **\-RunPluginsSerially**，  **\-StopOnPluginFailure**，并 **– SeparateReboots**参数。 有关使用多个插件的详细信息\-项中，使用下表中的 cmdlet 文档提供的链接。  
  
|Cmdlet|描述|  
|----------|---------------|  
|[Add-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/add-cauclusterrole)|添加 CAU 群集的角色提供自助\-到指定的群集更新的功能。|  
|[Invoke-CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-caurun)|为适用的更新执行群集节点的扫描并通过更新运行在指定的群集上安装这些更新。|  
|[Invoke-CauScan](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-causcan)|为适用的更新执行群集节点的扫描，并返回将应用于指定群集中每个节点的更新的初始集列表。|  
|[Set-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/set-cauclusterrole)|为指定群集上的 CAU 群集角色设置配置属性。|  
  
如果未指定 CAU 插件\-中通过使用这些 cmdlet 的参数，默认值是即插即用\-中**Microsoft.WindowsUpdatePlugin**。  
  
### <a name="specify-cau-plug-in-arguments"></a>指定 CAU 插入\-in 自变量  
在配置更新运行选项时，可以指定一个或多个*名称\=值*对\(参数\)为所选的即插即用\-中使用。 例如，在 CAU UI 中，你可以指定多个参数，如下所示：  
  
**Name1\=Value1;Name2\=Value2;Name3\=Value3**  
  
这些*名称\=值*对必须是有意义的即插即用\-中，您指定。 对于某些插件\-参数是可选的项。  
  
CAU 插件的语法\-in 自变量遵循以下通用规则：  
  
-   多个*名称\=值*对由分号分隔。  
  
-   包含空格的值括在引号中，例如：**Name1\="带有空格的值"**。  
  
-   确切语法*值*取决于即插即用\-中。  
  
若要指定即插即用\-中通过使用 CAU PowerShell cmdlet 支持的参数 **– CauPluginParameters**参数形式的参数传递：  
  
**\-CauPluginArguments @{Name1\=Value1;Name2\=Value2;Name3\=Value3}**  
  
此外可以使用预定义的 PowerShell 哈希表。 若要指定即插即用\-中的多个插件参数\-中，将传递的自变量，用逗号分隔的多个哈希表。 传递插头\-参数中即插即用\-以便在指定**CauPluginName**。  
  
### <a name="specify-optional-plug-in-arguments"></a>指定可选的即插即用\-in 自变量  
即插即用\-CAU 安装的项\( **Microsoft.WindowsUpdatePlugin**并**Microsoft.HotfixPlugin** \)提供可以选择的其他选项。 在 CAU UI 中，它们显示在**其他选项**页上配置的即插即用的更新运行选项之后\-中。 如果使用 CAU PowerShell cmdlet，这些选项被配置为可选的即插即用\-in 自变量。 有关详情，请参阅本主题后面的 [使用 Microsoft.WindowsUpdatePlugin](#BKMK_WUP) 和 [使用 Microsoft.HotfixPlugin](#BKMK_HFP) 。  
  
## <a name="manage-plug-ins-using-windows-powershell-cmdlets"></a>管理即插即用\-项使用 Windows PowerShell cmdlet  
  
|Cmdlet|描述|  
|----------|---------------|  
|[Get-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/get-cauplugin)|检索有关一个或多个软件更新插件的信息\-在本地计算机注册的项。|  
|[Register-CauPlugin]((https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin))|注册 CAU 软件更新插件\-在本地计算机上。|  
|[Unregister-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/unregister-cauplugin)|删除软件更新插件\-在从列表中的即插即用\-CAU 可以使用的项。 **注意：** 即插即用\-使用 CAU 安装的项\( **Microsoft.WindowsUpdatePlugin**并**Microsoft.HotfixPlugin** \)无法被注销。|  
  
## <a name="BKMK_WUP"></a>使用 Microsoft.WindowsUpdatePlugin  

默认插件\-中为 CAU， **Microsoft.WindowsUpdatePlugin**，执行以下操作：
- 与每个故障转移群集节点上的 Windows 更新代理通信，以应用在每个节点上运行的 Microsoft 产品所需的更新。
- 安装群集更新直接从 Windows Update 或 Microsoft Update，或从上\-本地 Windows Server Update Services \(WSUS\)服务器。
- 仅安装所选、 常规分发版本\(GDR\)更新。 默认情况下，即插即用\-中仅重要软件更新应用。 不需要任何配置。 默认配置在每个节点上下载并安装重要的 GDR 更新。 

> [!NOTE]
> 若要应用默认情况下选择的重要软件更新以外的更新\(例如，驱动程序更新\)，可以配置可选的即插即用\-参数中。 有关详细信息，请参阅[配置 Windows 更新代理查询字符串](#BKMK_QUERY)。

### <a name="requirements"></a>要求

- 故障转移群集和远程更新协调器计算机\(如果使用\)必须满足 CAU 和配置所需的远程管理中列出的要求[要求和适用于 CAU 的最佳做法](cluster-aware-updating-requirements.md).
- 查看[对应用 Microsoft 更新的建议](cluster-aware-updating-requirements.md#BKMK_BP_WUA)，然后对你的故障转移群集节点的 Microsoft 更新配置进行任何必要的更改。
- 为获得最佳结果，我们建议你运行 CAU 最佳做法分析器\(BPA\)来确保正确配置群集和更新环境通过使用 CAU 应用更新。 有关详细信息，请参阅[测试 CAU 更新的准备情况](cluster-aware-updating-requirements.md#BKMK_BPA)。

> [!NOTE]
> 排除需要接受 Microsoft 许可条款或需要用户交互的更新，并且必须手动安装更新。

### <a name="additional-options"></a>其他选项

或者，您可以指定以下插件\-参数来增加或限制的应用的即插即用的更新集\-中：
- 若要配置插件\-以应用建议的更新，除了在 CAU UI 中，每个节点上的重要更新**其他选项**页上，选择**让我建议更新相同的方式我接收重要更新**复选框。
<br>或者，配置 **'IncludeRecommendedUpdates'\='True'** 插入\-参数中。
- 若要配置插件\-中要筛选的应用于每个群集节点的 GDR 更新类型，指定 Windows 更新代理查询字符串 using **QueryString**插入\-参数中。 有关详细信息，请参阅[配置 Windows 更新代理查询字符串](#BKMK_QUERY)。

### <a name="BKMK_QUERY"></a>配置 Windows 更新代理查询字符串  
可以配置即插即用\-默认值的参数中插入\-在中， **Microsoft.WindowsUpdatePlugin**，其中包含 Windows 更新代理\(WUA\)查询字符串。 根据特定的选择条件，此指令使用 WUA API 来标识一个或多个组的 Microsoft 更新，以便应用到每个节点。 你可以通过使用逻辑“与”或者逻辑“或”组合多个条件。 即插即用中指定 WUA 查询字符串\-in 自变量，如下所示：  
  
**查询字符串\="Criterion1\=Value1 和\/或 Criterion2\=Value2 和\/或..."**  
  
例如，通过使用默认的 **Microsoft.WindowsUpdatePlugin** 参数，**QueryString** 自动选择重要更新，该默认参数使用 **IsInstalled**、**Type**、**IsHidden** 和 **IsAssigned** 条件构造：  
  
**查询字符串\="IsInstalled\=0 和类型\=软件和 IsHidden\=0 和 IsAssigned\=1"**  
  
如果指定**QueryString**参数，它用于替换默认**QueryString**配置的即插即用\-中。  
  
#### <a name="example-1"></a>示例 1
  
若要配置**QueryString**安装特定更新由 ID 标识的参数*f6ce46c1\-971 c\-43f9\-a2aa\-783df125f003*:  
  
**查询字符串\="UpdateID\=f6ce46c1\-971 c\-43f9\-a2aa\-783df125f003 和 IsInstalled\=0"**  
  
> [!NOTE]  
> 前面的示例适用于通过使用群集应用更新\-注意更新向导。 如果你想要通过配置自安装某个特定的更新\-更新选项，使用 CAU UI 或使用**添加\-CauClusterRole**或**设置\-CauClusterRole**PowerShell cmdlet，你必须使用两个单 UpdateID 值的格式\-引号字符：  
>   
> **QueryString\="UpdateID\=''f6ce46c1\-971c\-43f9\-a2aa\-783df125f003'' and IsInstalled\=0"**  
  
#### <a name="example-2"></a>示例 2
  
若要配置仅安装驱动程序的 **QueryString** 参数：  
  
**查询字符串\="IsInstalled\=0 和类型\=驱动程序和 IsHidden\=0"**  
  
为默认值的查询字符串的详细信息插入\-在中， **Microsoft.WindowsUpdatePlugin**，搜索条件\(如**IsInstalled**\)，可以在查询字符串中包含的语法，请参阅中有关搜索条件的部分和[Windows 更新代理 (WUA) API 参考](https://go.microsoft.com/fwlink/p/?LinkId=223304)。  
  
## <a name="BKMK_HFP"></a>使用 Microsoft.HotfixPlugin  
即插即用\-中**Microsoft.HotfixPlugin**可用于应用 Microsoft 有限分发版本\(LDR\)更新\(也称为修补程序，以前称为 Qfe\) ，你独立下载以解决特定的 Microsoft 软件问题。 插件从 SMB 文件共享上的根文件夹中安装更新，也可以自定义应用非\-Microsoft 驱动程序、 固件和 BIOS 更新。

> [!NOTE]
> 修补程序有时可供从 Microsoft 下载的知识库文章，但它们也将提供给客户作为\-所需的基础。

### <a name="requirements"></a>要求

- 故障转移群集和远程更新协调器计算机\(如果使用\)必须满足 CAU 和配置所需的远程管理中列出的要求[要求和适用于 CAU 的最佳做法](cluster-aware-updating-requirements.md).
- 查看[使用 Microsoft.HotfixPlugin 的建议](cluster-aware-updating-requirements.md#BKMK_BP_HF)。
- 为获得最佳结果，我们建议你运行 CAU 最佳做法分析器\(BPA\)模型，以确保正确配置群集和更新环境通过使用 CAU 应用更新。 有关详细信息，请参阅[测试 CAU 更新的准备情况](cluster-aware-updating-requirements.md#BKMK_BPA)。
- 从发布服务器，获取更新并将其复制或将其提取到服务器消息块\(SMB\)文件共享\(修补程序根文件夹\)至少支持 SMB 2.0，这是所有群集可以访问节点和远程更新协调器计算机\(如果在远程使用 CAU\-更新模式\)。 有关详细信息，请参阅本主题后面部分中的[配置修补程序根文件夹结构](#BKMK_HF_ROOT)。 

    > [!NOTE]
    > 默认情况下，该插件\-中仅安装修补程序具有以下文件扩展名：.msu、.msi 和.msp。

- 将 DefaultHotfixConfig.xml 文件复制\(中提供 **%systemroot%\\System32\\WindowsPowerShell\\v1.0\\模块\\ClusterAwareUpdating**的计算机上安装 CAU 工具的文件夹\)创建修补程序根文件夹，并且在其中提取了修补程序。 例如，将复制到配置文件 *\\ \\MyFileServer\\修补程序\\根\\*。 

    > [!NOTE]
    > 若要安装由 Microsoft 和其他更新提供的大部分修补程序，可以使用默认修补程序配置文件而无需任何修改。 如果你的方案需要它，你可以将配置文件作为一项高级任务进行自定义。 例如，配置文件可以包含自定义规则，以便处理具有特定扩展名的修补程序文件或定义特定退出条件的行为。 有关详细信息，请参阅本主题后面部分中的[自定义修补程序配置文件](#BKMK_CONFIG_FILE)。

### <a name="configuration"></a>配置

配置以下设置。 有关详细信息，请参阅本主题中后面部分的链接。
- 包含要应用的更新以及修补程序配置文件的共享修补程序根文件夹的路径。 可以在 CAU UI 中键入该路径，也可以配置**HotfixRootFolderPath\=\<路径 >** PowerShell 即插即用\-参数中。 

   > [!NOTE]
   > 您可以指定修补程序根文件夹为本地文件夹路径或 UNC 路径的窗体 *\\ \\ServerName\\共享\\RootFolderName*。 域\-基于或可以使用独立 DFS Namespace 路径。 但是，即插即用\-中检查访问权限的功能修补程序配置文件中的权限与不兼容的 DFS Namespace 路径，因此如果你配置一个，通过使用 CAU UI 或通过配置，则必须禁用访问权限的检查**DisableAclChecks\='True'** 插入\-参数中。
- 托管修补程序根文件夹的相应权限访问该文件夹，并确保从 SMB 访问的数据的完整性检查的服务器上设置共享文件夹\(SMB 签名或 SMB 加密\)。 有关详细信息，请参阅 [限制对修补程序根文件夹的访问权限](#BKMK_ACL)。

### <a name="additional-options"></a>其他选项

- （可选） 配置即插即用\-中因此该强制进行 SMB 加密时访问修补程序文件共享中的数据。 在 CAU UI 中，在**其他选项**页上，选择**要求在访问修补程序根文件夹中的 SMB 加密**选项，或配置**RequireSMBEncryption\='True'** PowerShell 即插即用\-参数中。 
  > [!IMPORTANT]
  > 若要借助 SMB 签名或 SMB 加密启用 SMB 数据的完整性，则必须在 SMB 服务器上执行额外的配置步骤。 有关详细信息，请参阅[限制对修补程序根文件夹的访问权限](#BKMK_ACL)中的第 4 步。 如果你选择强制使用 SMB 加密选项，并且修补程序根文件夹未配置为使用 SMB 加密进行访问，则更新运行将失败。
- 或者，禁用针对修补程序根文件夹的足够权限和修补程序配置文件的默认检查。 在 CAU UI 中，选择**禁用对修补程序根文件夹和配置文件管理员访问权限检查**，或配置**DisableAclChecks\='True'** 插入\-中自变量。
- （可选） 配置**HotfixInstallerTimeoutMinutes\= <Integer>** 参数来指定多长时间的修补程序插入\-中等待该修补程序安装程序进程返回。 \(默认值为 30 分钟。\)例如，若要指定两个小时的超时周期，设置**HotfixInstallerTimeoutMinutes\=120**。
- （可选） 配置**HotfixConfigFileName \= <name>** 插入\-参数以指定位于修补程序根文件夹中的修补程序配置文件名称中。 如果未指定，则使用默认名称 DefaultHotfixConfig.xml。
  
### <a name="BKMK_HF_ROOT"></a>配置修补程序根文件夹结构

为了使修补程序插入\-中以起作用，修补程序必须存储在答是这样\-SMB 文件共享中定义结构\(修补程序根文件夹\)，并且必须配置修补程序插件\-使用的路径通过使用 CAU UI 或 CAU PowerShell cmdlet 的修补程序根文件夹。 此路径传递给插头\-中作为**HotfixRootFolderPath**参数。 如以下示例所示，根据更新需要，你可以为修补程序根文件夹选择多个结构中的一个。 不符合该结构的文件或文件夹将会忽略。  
  
#### <a name="example-1---folder-structure-used-to-apply-hotfixes-to-all-cluster-nodes"></a>示例 1-文件夹结构用于将修补程序应用到所有群集节点
  
若要指定修补程序适用于所有群集节点，将它们复制到名为的文件夹**CAUHotfix\_所有**修补程序根文件夹下。 在此示例中， **HotfixRootFolderPath**插入\-参数中设置为 *\\ \\MyFileServer\\修补程序\\根\\*. **CAUHotfix\_所有**文件夹包含三个更新扩展.msu、.msi 和.msp 将应用于所有群集节点。 更新文件名称仅用于说明目的。  
  
> [!NOTE]  
> 在此示例和以下示例中，具有默认名称 DefaultHotfixConfig.xml 的修补程序配置文件在修补程序根文件夹中其所需的位置中显示。  
  
```
\\MyFileServer\Hotfixes\Root\   
   DefaultHotfixConfig.xml  
   CAUHotfix_All\   
      Update1.msu   
      Update2.msi   
      Update3.msp  
      ...  
```  
  
#### <a name="example-2---folder-structure-used-to-apply-certain-updates-only-to-a-specific-node"></a>示例 2-文件夹结构用于某些更新仅适用于特定节点
  
若要指定仅适用于特定节点的修补程序，请在具有节点名称的修补程序根文件夹下使用子文件夹。 使用群集节点的 NetBIOS 名称，例如，*ContosoNode1*。 然后，将仅适用于此节点的更新移动到该子文件夹。 在以下示例中， **HotfixRootFolderPath**插入\-参数中设置为 *\\ \\MyFileServer\\修补程序\\根\\*. 中的更新**CAUHotfix\_所有**文件夹将应用于所有群集节点，和*Node1\_特定\_Update.msu*将仅应用于*ContosoNode1*。  
  
```
\\MyFileServer\Hotfixes\Root\   
   DefaultHotfixConfig.xml  
   CAUHotfix_All\   
      Update1.msu   
      Update2.msi   
      Update3.msp  
      ...  
   ContosoNode1\   
      Node1_Specific_Update.msu   
      ...  
```  
  
#### <a name="example-3---folder-structure-used-to-apply-updates-other-than-msu-msi-and-msp-files"></a>示例 3-文件夹结构用于应用.msu、.msi 和.msp 文件以外的更新
  
默认情况下，**Microsoft.HotfixPlugin** 仅将应用具有 .msu、.msi 或 .msp 扩展名的更新。 但是，某些更新可能具有不同的扩展名，并且需要不同的安装命令。 例如，你可能需要将具有 .exe 扩展名的固件更新应用于群集中的节点。 您可以使用表示非特定的子文件夹配置修补程序根文件夹\-应安装默认更新类型。 你还必须配置相应的文件夹安装规则，该规则用于指定修补程序配置 XML 文件中的 `<FolderRules>` 元素内的安装命令。  
  
在以下示例中， **HotfixRootFolderPath**插入\-参数中设置为 *\\ \\MyFileServer\\修补程序\\根\\*. 多个更新将应用于所有群集节点，并且通过使用 *FolderRule1* 将固件更新 *SpecialHotfix1.exe* 应用于 *ContosoNode1*。 有关在修补程序配置文件中配置 *ContosoNode1* 的详细信息，请参阅本主题后面部分中的 [自定义修补程序配置文件](#BKMK_CONFIG_FILE) 。  
  
```
\\MyFileServer\Hotfixes\Root\   
   DefaultHotfixConfig.xml  
   CAUHotfix_All\   
      Update1.msu   
      Update2.msi   
      Update3.msp  
      ...  
  
   ContosoNode1\   
      FolderRule1\  
          SpecialHotfix1.exe  
      ...  
```

### <a name="BKMK_CONFIG_FILE"></a>自定义修补程序配置文件  
修补程序配置文件控制 **Microsoft.HotfixPlugin** 如何在故障转移群集中安装特定修补程序文件类型。 在 HotfixConfigSchema.xsd 文件内定义的配置文件的 XML 架构，该文件位于安装 CAU 工具的计算机上的以下文件夹中：  
  
**%systemroot%\\System32\\WindowsPowerShell\\v1.0\\Modules\\ClusterAwareUpdating folder**  
  
若要自定义修补程序配置文件，将从该位置的示例配置文件 DefaultHotfixConfig.xml 复制到修补程序根文件夹并为你的方案进行适当的修改。  
  
> [!IMPORTANT]  
> 若要应用由 Microsoft 和其他更新所提供的大部分修补程序，可以使用默认修补程序配置文件而无需任何修改。 自定义修补程序配置文件是仅在高级使用方案中才出现的任务。  
  
默认情况下，修补程序配置 XML 文件定义安装规则和针对以下两个修补程序类别的退出条件：  
  
-   具有扩展名的修补程序文件的即插即用\-在可以安装默认情况下\(.msu、.msi 和.msp 文件\)。  
  
    它们定义为 `<ExtensionRules>` 元素中的 `<DefaultRules>` 元素。 还有一个用于每个默认支持文件类型的 `<Extension>` 元素。 常规 XML 结构如下所示：  
  
    ```xml  
    <DefaultRules>  
        <ExtensionRules>  
          <Extension name="MSI">  
            <!-- Template and ExitConditions elements for installation of .msi files follow -->  
             ...  
          </Extension>  
          <Extension name="MSU">  
            <!-- Template and ExitConditions elements for installation of .msu files follow -->  
             ...  
          </Extension>  
          <Extension name="MSP">  
            <!-- Template and ExitConditions elements for installation of .msp files follow -->  
             ...  
          </Extension>  
             ...  
       </ExtensionRules>  
    </DefaultRules>  
    ```  
  
    如果你需要将某些更新类型应用到你的环境中的所有群集节点，则可以定义其他 `<Extension>` 元素。  
  
-   修补程序或其他更新文件不是.msi、.msu 或.msp 文件，例如，非\-Microsoft 驱动程序、 固件和 BIOS 更新。  
  
    每个非\-默认文件类型配置为`<Folder>`中的元素`<FolderRules>`元素。 `<Folder>` 元素的名称属性必须与将包含相应类型更新的修补程序根文件夹中文件夹的名称相同。 文件夹可以位于**CAUHotfix\_所有**文件夹或节点中\-特定文件夹。 例如，如果在修补程序根文件夹中配置 *FolderRule1* ，则在 XML 文件中配置以下元素以定义安装模板和该文件夹中更新的退出条件：  
  
    ```xml  
    <FolderRules>  
          <Folder name="FolderRule1">  
            <!-- Template and ExitConditions elements for installation of updates in FolderRule1 follow -->  
             ...  
          </Folder>  
          ...  
    </FolderRules>  
    ```  
  
下表描述了 `<Template>` 属性和可能的 `<ExitConditions>` 子元素。  
  
|`<Template>` 属性|描述|  
|--------------------------|---------------|  
|`path`|在 `<Extension name>` 属性中所定义文件类型的安装程序的完整路径。<br /><br />若要指定修补程序根文件夹结构中的更新文件的路径，请使用 `$update$`。|  
|`parameters`|在 `path`中指定的程序的必需和可选参数字符串。<br /><br />若要指定一个作为修补程序根文件夹结构中的更新文件路径的参数，请使用 `$update$`。|  
  
|`<ExitConditions>` 子元素|描述|  
|---------------------------------|---------------|  
|`<Success>`|定义一个或多个表明指定更新已成功的退出代码。 这是必需的子元素。|  
|`<Success_RebootRequired>`|或者，定义一个或多个退出代码，以表明指定的更新已成功，并且节点必须重新启动。 <br>**注意：** 或者，`<Folder>` 元素可以包含 `alwaysReboot` 属性。 如果设置此属性，它表明如果按照此规则安装修补程序，则返回在 `<Success>`中定义的退出代码之一，此情况可以解释为 `<Success_RebootRequired>` 退出条件。|  
|`<Fail_RebootRequired>`|或者，定义一个或多个退出代码，以表明指定的更新已失败，并且节点必须重新启动。|  
|`<AlreadyInstalled>`|或者，定义一个或多个退出代码，以表明不应用指定的更新，因为它已经安装。|  
|`<NotApplicable>`|或者，定义一个或多个退出代码，以表明不应用指定的更新，因为它不适用于群集节点。|  
  
> [!IMPORTANT]  
> 任何未在 `<ExitConditions>` 中明确定义的退出代码解释为更新失败，并且不重新启动节点。  
  
### <a name="BKMK_ACL"></a>限制对修补程序根文件夹的访问  
你必须执行几个步骤来配置 SMB 文件服务器和文件共享以帮助确保仅在 **Microsoft.HotfixPlugin** 的上下文中对修补程序根文件夹的文件和修补程序配置文件进行访问。 这些步骤启用多个功能，从而帮助防止可能在某种程度上危及故障转移群集的修补程序文件篡改。  
  
常规步骤如下所示：  
  
1.  标识用于更新运行通过使用即插即用的用户帐户\-中  
  
2.  在 SMB 文件服务器上必要的组中配置此用户帐户  
  
3.  配置权限以访问修补程序根文件夹  
  
4.  配置 SMB 数据完整性的设置  
  
5.  在 SMB 服务器上启用 Windows 防火墙规则  
  
#### <a name="step-1-identify-the-user-account-that-is-used-for-updating-runs-by-using-the-hotfix-plug-in"></a>步骤 1： 标识用于更新运行通过使用修补程序插件的用户帐户\-中
  
在 CAU 中用于执行更新运行使用时检查安全设置的帐户**Microsoft.HotfixPlugin**取决于是否在远程使用 CAU\-更新模式或 self\-更新模式下，作为如下所示：  
  
-   **远程\-更新模式**预览并应用更新的群集具有管理权限的帐户。  
  
-   **自助\-更新模式**群集角色在 Active Directory 中为 CAU 配置的虚拟计算机对象的名称。 这是为 CAU 群集角色在 Active Directory 中预留的虚拟计算机对象的名称或由 CAU 为群集角色生成的名称。 若要获取的名称，则由 CAU 生成，请运行**获取\-CauClusterRole** CAU PowerShell cmdlet。 在输出中，**ResourceGroupName** 是生成的虚拟计算机对象帐户的名称。  
  
#### <a name="step-2-configure-this-user-account-in-the-necessary-groups-on-an-smb-file-server"></a>步骤 2： 在 SMB 文件服务器上必要的组中配置此用户帐户
  
> [!IMPORTANT]  
> 你必须在 SMB 服务器上作为本地管理员帐户添加用于更新运行的帐户。 如果因为你组织中的安全策略而不允许这样做，则通过以下过程在 SMB 服务器上使用必要的权限来配置此帐户。  
  
##### <a name="to-configure-a-user-account-on-the-smb-server"></a>在 SMB 服务器上配置用户帐户  
  
1.  将用于更新运行的帐户添加到 Distributed COM Users 组和以下任一组：Power User、服务器操作或打印操作员。  
  
2.  若要启用帐户所需的 WMI 权限，请在 SMB 服务器上启动 WMI 管理控制台。 启动 PowerShell，然后键入以下命令：  
  
    ```  
    wmimgmt.msc  
    ```  
  
3.  在控制台树中，右键\-单击**WMI 控件\(本地\)**，然后单击**属性**。  
  
4.  单击“安全”，然后展开“根”。  
  
5.  单击“CIMV2” ，然后单击“安全” 。  
  
6.  将用于更新运行的帐户添加到“组或用户名”列表。  
  
7.  将“执行方法”  和“远程启用”  权限授予用于更新运行的帐户。  
  
#### <a name="step-3-configure-permissions-to-access-the-hotfix-root-folder"></a>步骤 3： 配置权限以访问修补程序根文件夹
  
默认情况下，当你尝试应用更新，修补程序插入\-中检查访问修补程序根文件夹的 NTFS 文件系统权限的配置。 如果文件夹访问权限配置不正确，更新运行使用修补程序插件\-中可能会失败。  
  
如果你使用修补程序插件的默认配置\-中，确保文件夹访问权限满足以下要求。  
  
-   用户组具有读取权限。  
  
-   如果插件\-在将应用具有.exe 扩展名的更新、 用户组具有执行权限。  
  
-   允许仅特定安全主体\(但不是必需\)具有写入或修改权限。 允许的主体是本地管理员组、SYSTEM、CREATOR OWNER 和 TrustedInstaller。 其他帐户或组不允许具有写入或修改修补程序根文件夹的权限。  
  
（可选） 可以禁用的前面的检查，即插即用\-中默认情况下执行。 可以通过两种方式之一完成此操作：  
  
-   如果使用 CAU PowerShell cmdlet，配置**DisableAclChecks\='True'** 中的参数**CauPluginArguments**修补程序插件参数\-中。  
  
-   如果使用 CAU UI，请选择用于配置更新运行选项的向导中“其他更新选项”页面上的“禁用对修补程序根文件夹和配置文件管理员访问权限的检查”选项。  
  
但是，作为在许多环境中的最佳实践，我们建议你使用默认的配置强制执行这些检查。  
  
#### <a name="step-4-configure-settings-for-smb-data-integrity"></a>步骤 4： 配置 SMB 数据完整性的设置
  
若要检查群集节点和 SMB 文件共享之间的连接中的数据完整性，修补程序插入\-中，您需要启用 SMB 签名或 SMB 加密的 SMB 文件共享上的设置。 SMB 加密，提供了增强的安全性和更好的性能，在许多环境中，开始 Windows Server 2012 中支持。 你可以启用其中一种设置或同时启用这两种设置，如下所示：  
  
-   若要启用 SMB 签名，请参阅 Microsoft 知识库内的 [文章 887429](https://support.microsoft.com/kb/887429) 中描述的过程。  
  
-   若要为 SMB 共享文件夹启用 SMB 加密，请在 SMB 服务器上运行以下 PowerShell cmdlet:  
  
    ```PowerShell  
    Set-SmbShare <ShareName> -EncryptData $true  
    ```  
  
    其中 <*ShareName*> 是 SMB 共享文件夹的名称。  
  
（可选） 若要强制使用 SMB 加密在 SMB 服务器的连接中，选择**要求在访问修补程序根文件夹中的 SMB 加密**选项在 CAU UI 中，或配置**RequireSMBEncryption\='True'** 插入\-中使用 CAU PowerShell cmdlet 的参数。  
  
> [!IMPORTANT]  
> 如果你选择强制使用 SMB 加密的选项，并且修补程序根文件夹未配置为使用 SMB 加密的连接，则更新运行将失败。  
  
#### <a name="step-5-enable-a-windows-firewall-rule-on-the-smb-server"></a>步骤 5： 在 SMB 服务器上启用 Windows 防火墙规则
  
必须启用**文件服务器远程管理\(SMB\-中\)** SMB 文件服务器上 Windows 防火墙中的规则。 这是 Windows Server 2016、 Windows Server 2012 R2 和 Windows Server 2012 中默认情况下启用的。  
  
## <a name="see-also"></a>请参阅  
  
-   [群集感知更新概述](cluster-aware-updating.md)
  
-   [群集感知更新 Windows PowerShell Cmdlet](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating)  
  
-   [群集感知更新插件参考](https://msdn.microsoft.com/library/hh418084.aspx)  
  
