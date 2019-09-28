---
ms.assetid: d44d4430-41e2-483a-9685-92610cdef32a
title: 群集感知更新插件的工作原理
ms.topic: article
ms.prod: windows-server
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 04/28/2017
ms.technology: storage-failover-clustering
description: 当使用 Windows Server 中的群集感知更新在群集上安装更新时，如何使用插件协调更新。
ms.openlocfilehash: f6c572a397530704dd91d9c67c5c1758ccc085c4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361287"
---
# <a name="how-cluster-aware-updating-plug-ins-work"></a>群集感知更新插件的工作原理

>适用于：Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

[群集感知更新](cluster-aware-updating.md)（CAU）使用插件跨故障转移群集中的节点协调更新的安装。 本主题提供有关使用为 CAU 安装的内置 @ no__t-0in CAU 插件 @ no__t-1ins 或其他插件 @ no__t-2ins 的信息。

## <a name="BKMK_INSTALL"></a>安装 a @ no__t-1in  
除了默认的插件 @ no__t-1ins 以外，还必须单独安装 CAU \(**microsoft.windowsupdateplugin**和**microsoft.hotfixplugin**@no__t 的插件 @ no__t-0in。 如果在 no__t-0updating 模式下使用 CAU，则必须在所有群集节点上安装插件 @ no__t-1in。 如果在远程 @ no__t-0updating 模式下使用 CAU，则必须在远程更新协调器计算机上安装插件 @ no__t-1in。 在每个节点上安装的 no__t-0in 可能有其他安装要求。  
  
若要安装 a @ no__t-0in，请按照来自插件 @ no__t-1in 发布服务器的说明进行操作。 若要使用 CAU 手动注册插件 @ no__t-0in，请在安装了插件 @ no__t-2in 的每台计算机上运行[register-cauplugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin) cmdlet。  
  
## <a name="specify-a-plug-in-and-plug-in-arguments"></a>指定 a @ no__t-0in 和 a @ no__t-1in 参数  
  
### <a name="specify-a-cau-plug-in"></a>指定 CAU 插件 @ no__t-0in

在 CAU UI 中，当你使用 CAU 来执行以下操作时，可以从可用插件 @ no__t-2ins 的 "删除 @ no__t-1down" 列表中选择 "a @ no__t-0in"。  
  
-   将更新应用到群集  
  
-   群集的预览更新  
  
-   配置群集 self @ no__t-0updating 选项  
  
默认情况下，CAU 会选择插件 @ no__t-0in **microsoft.windowsupdateplugin**。 不过，你可以指定任何已安装并向 CAU 注册的插件 @ no__t-0in。

> [!TIP]  
> 在 CAU UI 中，你只能为 CAU 指定一个用于预览的即插即用的0in，或在更新运行过程中应用更新。 通过使用 CAU PowerShell cmdlet，可以指定一个或多个 @ no__t-0ins。如果你需要在群集上安装多个类型的更新，则在一个更新运行中指定多个插件 @ no__t-0ins 通常更高效，而不是对每个插入 @ no__t-1in 使用单独的更新运行。 例如，通常会发生更少的节点重新启动。

通过使用下表中列出的 CAU PowerShell cmdlet，可以通过传递 **– CauPluginName**参数为更新运行或扫描指定一个或多个即插 @ no__t-0ins。 可以指定多个 @ no__t-0in 名称，方法是使用逗号分隔这些名称。 如果指定多个插件 @ no__t-0ins，还可以通过指定 **\-RunPluginsSerially**、 **\-StopOnPluginFailure**和 **– SeparateReboots，来控制在更新运行期间，即插入 @ no__t 1ins 的影响情况。** 参数。 若要详细了解如何使用多个插件 @ no__t-0ins，请使用下表中的 cmdlet 文档所提供的链接。  
  
|Cmdlet|描述|  
|----------|---------------|  
|[Add-cauclusterrole](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/add-cauclusterrole)|将向指定群集提供自我 @ no__t-0updating 功能的 CAU 群集角色。|  
|[Invoke-caurun](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/invoke-caurun)|为适用的更新执行群集节点的扫描并通过更新运行在指定的群集上安装这些更新。|  
|[Invoke-causcan](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/invoke-causcan)|为适用的更新执行群集节点的扫描，并返回将应用于指定群集中每个节点的更新的初始集列表。|  
|[Add-cauclusterrole](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/set-cauclusterrole)|为指定群集上的 CAU 群集角色设置配置属性。|  
  
如果未使用这些 cmdlet 指定 CAU 插件 @ no__t-0in 参数，则默认值为即插 @ no__t-1in **microsoft.windowsupdateplugin**。  
  
### <a name="specify-cau-plug-in-arguments"></a>指定 CAU 插件 @ no__t-0in 参数  
在配置更新运行选项时，可以为所选的 @ no__t-4in 指定一个或多个 *@ no__t @no__t-* 2arguments @ no__t-@。 例如，在 CAU UI 中，你可以指定多个参数，如下所示：  
  
**Name1 @ no__t-1Value1;Name2 @ no__t-2Value2;Name3 @ no__t-3Value3**  
  
对于指定的插件 @ no__t-2in，这些*name @ no__t-1value*对必须是有意义的。 对于某些 "插入 @ no__t-0ins"，这些参数是可选的。  
  
CAU 插件 @ no__t-0in 参数的语法遵循以下通用规则：  
  
-   多个*名称 @ no__t-1value*对用分号分隔。  
  
-   包含空格的值括在引号中，例如：**Name1 @ no__t-1 "带有空格的值"** 。  
  
-   *值*的确切语法取决于插件 @ no__t-1in。  
  
若要使用支持 **– CauPluginParameters**参数的 CAU PowerShell cmdlet 指定 no__t-0in 参数，请传递以下形式的参数：  
  
**\-CauPluginArguments @ {Name1 @ no__t-2Value1;Name2 @ no__t-3Value2;Name3 @ no__t-4Value3}**  
  
你还可以使用预定义的 PowerShell 哈希表。 若要为多个 @ no__t-1in 指定 no__t-0in 参数，请传递多个用逗号分隔的哈希表。 在**CauPluginName**中指定的即插即用 @ no__t-1in 顺序传递 no__t-0in 参数。  
  
### <a name="specify-optional-plug-in-arguments"></a>指定可选的插件 @ no__t-0in 参数  
No__t 安装的即插即用的 0ins \(**microsoft.windowsupdateplugin**和**microsoft.hotfixplugin**\) 提供可选择的其他选项。 在 CAU UI 中，在为插件配置更新运行选项后，这些选项将显示在 "**其他选项**" 页上。 如果使用 CAU PowerShell cmdlet，则这些选项将配置为可选的插件 @ no__t-0in 参数。 有关详情，请参阅本主题后面的 [使用 Microsoft.WindowsUpdatePlugin](#BKMK_WUP) 和 [使用 Microsoft.HotfixPlugin](#BKMK_HFP) 。  
  
## <a name="manage-plug-ins-using-windows-powershell-cmdlets"></a>使用 Windows PowerShell cmdlet 管理插件 @ no__t-0ins  
  
|Cmdlet|描述|  
|----------|---------------|  
|[Register-cauplugin](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/get-cauplugin)|检索有关在本地计算机上注册的一个或多个软件更新插件 @ no__t-0ins 的信息。|  
|[注册-Register-cauplugin]((https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/register-cauplugin))|在本地计算机上注册 CAU 软件更新插件 @ no__t-0in。|  
|[取消注册-Register-cauplugin](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/unregister-cauplugin)|从 CAU 可以使用的插件 @ no__t-1ins 列表中删除软件更新插件 @ no__t-0in。 **注意：** 不能取消注册与 CAU \(**microsoft.windowsupdateplugin**和**microsoft.hotfixplugin**\) 一起安装的插件 @ no__t。|  
  
## <a name="BKMK_WUP"></a>使用 Microsoft.windowsupdateplugin  

对于 CAU **microsoft.windowsupdateplugin**，默认的即插即用 no__t-0in 会执行以下操作：
- 与每个故障转移群集节点上的 Windows 更新代理通信，以应用在每个节点上运行的 Microsoft 产品所需的更新。
- 直接从 Windows 更新或 Microsoft 更新或从 @ no__t-0premises Windows Server Update Services \(WSUS @ no__t 服务器安装群集更新。
- 仅安装所选的常规分发版本 \(GDR @ no__t 更新。 默认情况下，即插即用 no__t-0in 仅应用重要的软件更新。 不需要任何配置。 默认配置在每个节点上下载并安装重要的 GDR 更新。 

> [!NOTE]
> 若要应用除默认情况下所选的重要软件更新 \(for 示例，驱动程序更新 @ no__t）的更新，你可以配置一个可选的插件 @ no__t-2in 参数。 有关详细信息，请参阅[配置 Windows 更新代理查询字符串](#BKMK_QUERY)。

### <a name="requirements"></a>要求

- 使用的故障转移群集和远程更新协调器计算机 \(if 必须满足 CAU 的要求以及[cau 的要求和最佳做法](cluster-aware-updating-requirements.md)中列出的远程管理所需的配置。
- 查看[对应用 Microsoft 更新的建议](cluster-aware-updating-requirements.md#BKMK_BP_WUA)，然后对你的故障转移群集节点的 Microsoft 更新配置进行任何必要的更改。
- 为了获得最佳结果，我们建议你运行 CAU 最佳做法分析器 \(BPA @ no__t，以确保正确配置群集和更新环境，以便通过使用 CAU 来应用更新。 有关详细信息，请参阅[测试 CAU 更新的准备情况](cluster-aware-updating-requirements.md#BKMK_BPA)。

> [!NOTE]
> 排除需要接受 Microsoft 许可条款或需要用户交互的更新，并且必须手动安装更新。

### <a name="additional-options"></a>其他选项

或者，你可以指定以下 @ no__t-0in 参数以增加或限制由插件 @ no__t-1in 应用的更新集：
- 若要配置 no__t-0in 以应用建议的更新以及每个节点上的重要更新，请在 CAU UI 中的 "**其他选项**" 页上，选择 "**以接收重要更新的相同方式为我推荐更新"** 复选文本框.
<br>或者，配置 **"IncludeRecommendedUpdates" \= ' True '** a @ no__t-2in 参数。
- 若要配置 no__t-0in 以筛选应用于每个群集节点的 GDR 更新类型，请使用**QueryString** @ no__t-2in 参数指定 Windows 更新代理查询字符串。 有关详细信息，请参阅[配置 Windows 更新代理查询字符串](#BKMK_QUERY)。

### <a name="BKMK_QUERY"></a>配置 Windows 更新代理查询字符串  
你可以为默认的 "a @ no__t-1in， **microsoft.windowsupdateplugin**" 配置一个 "a @ no__t-0in" 参数，该参数包含一个 Windows 更新代理 \(WUA @ no__t-4 查询字符串。 根据特定的选择条件，此指令使用 WUA API 来标识一个或多个组的 Microsoft 更新，以便应用到每个节点。 你可以通过使用逻辑“与”或者逻辑“或”组合多个条件。 在插件 @ no__t-0in 参数中指定 WUA 查询字符串，如下所示：  
  
**QueryString @ no__t-1 "Criterion1 @ no__t-2Value1 and @ no__t-3or Criterion2 @ no__t-4Value2 and @ no__t-5or ..."**  
  
例如，通过使用默认的 **Microsoft.WindowsUpdatePlugin** 参数，**QueryString** 自动选择重要更新，该默认参数使用 **IsInstalled**、**Type**、**IsHidden** 和 **IsAssigned** 条件构造：  
  
**QueryString @ no__t-1 "IsInstalled @ no__t，Type @ no__t-3'Software ' and IsHidden @ no__t-40 and IsAssigned @ no__t-51"**  
  
如果指定**QueryString**参数，则将使用该参数来替换为即插 @ no__t-2in 配置的默认**查询字符串**。  
  
#### <a name="example-1"></a>示例 1
  
配置一个**查询字符串**参数，该参数将安装由 ID f6ce46c1 @ *no__t-2971c @ no__t-343f9 @ no__t-4a2aa @ no__t-* 5783df125f003 标识的特定更新：  
  
**QueryString @ no__t-1 "UpdateID @ no__t-2'f6ce46c1 @ no__t-3971c @ no__t-443f9 @ no__t-5a2aa @ no__t-6783df125f003 ' and IsInstalled @ no__t-70"**  
  
> [!NOTE]  
> 前面的示例适用于使用 Cluster @ no__t-0Aware 更新向导应用更新。 如果要通过使用 CAU UI 配置 no__t-0updating 选项或使用**Add @ no__t-2CauClusterRole**或**Set @ no__t-4CauClusterRole**PowerShell cmdlet 来安装特定更新，则必须使用两个选项来设置 UpdateID 值的格式：单个 @ no__t-5quote 字符：  
>   
> **QueryString @ no__t-1 "UpdateID @ no__t-2''f6ce46c1 @ no__t-3971c @ no__t-443f9 @ no__t-5a2aa @ no__t-6783df125f003 ' ' and IsInstalled @ no__t-70"**  
  
#### <a name="example-2"></a>示例 2
  
若要配置仅安装驱动程序的 **QueryString** 参数：  
  
**QueryString @ no__t-1 "IsInstalled @ no__t-20 and Type @ no__t-3'Driver ' and IsHidden @ no__t-40"**  
  
有关默认的插件 @ no__t-0in， **microsoft.windowsupdateplugin**，搜索条件 \(Such 作为**IsInstalled**\) 的查询字符串的详细信息，以及查询字符串中可以包含的语法，请参阅部分关于[Windows 更新 Agent （WUA） API 参考](https://go.microsoft.com/fwlink/p/?LinkId=223304)中的搜索条件。  
  
## <a name="BKMK_HFP"></a>使用 Microsoft.hotfixplugin  
可以使用 no__t-0in **microsoft.hotfixplugin**将 microsoft 有限分发版 \(LDR @ no__t-3 updates \(also 称为修补程序，以前称为 qfe @ no__t-5，它们分别下载到特定的 Microsoft 软件问题。 插件从 SMB 文件共享上的根文件夹安装更新，还可以自定义以应用非 @ no__t-0Microsoft 驱动程序、固件和 BIOS 更新。

> [!NOTE]
> 在知识库文章中，有时可以从 Microsoft 下载修补程序，但这些修补程序也以 @ no__t-0needed 的形式提供给客户。

### <a name="requirements"></a>要求

- 使用的故障转移群集和远程更新协调器计算机 \(if 必须满足 CAU 的要求以及[cau 的要求和最佳做法](cluster-aware-updating-requirements.md)中列出的远程管理所需的配置。
- 查看[使用 Microsoft.HotfixPlugin 的建议](cluster-aware-updating-requirements.md#BKMK_BP_HF)。
- 为了获得最佳结果，我们建议你运行 CAU 最佳做法分析器 \(BPA @ no__t 模型，以确保正确配置群集和更新环境，以便通过使用 CAU 来应用更新。 有关详细信息，请参阅[测试 CAU 更新的准备情况](cluster-aware-updating-requirements.md#BKMK_BPA)。
- 获取发布服务器中的更新，并将其复制或提取到服务器消息块 \(SMB @ no__t 文件共享 \(hotfix 根文件夹 @ no__t-3 至少支持 SMB 2.0，并且可由所有群集节点和远程更新访问协调器计算机 \(if CAU 用于远程 @ no__t-5updating 模式 @ no__t。 有关详细信息，请参阅本主题后面部分中的[配置修补程序根文件夹结构](#BKMK_HF_ROOT)。 

    > [!NOTE]
    > 默认情况下，此插件 @ no__t-0in 仅安装具有以下文件扩展名的修补程序： .msu、.msi 和 .msp。

- 将 Defaulthotfixconfig.xml 文件复制到 **% systemroot% \\System32 @ no__t-3WindowsPowerShell @ no__t-4v 1.0 @ no__t-** 5Modules @ no__t 文件夹中的 @no__t 文件，该计算机上的 CAU 工具是已将 @ no__t 安装到你创建的修补程序根文件夹中，并在其中提取了修补程序。 例如，将配置文件复制到 *\\ @ no__t-2MyFileServer @ no__t-3Hotfixes @ no__t-4Root @ no__t-5*。 

    > [!NOTE]
    > 若要安装由 Microsoft 和其他更新提供的大部分修补程序，可以使用默认修补程序配置文件而无需任何修改。 如果你的方案需要它，你可以将配置文件作为一项高级任务进行自定义。 例如，配置文件可以包含自定义规则，以便处理具有特定扩展名的修补程序文件或定义特定退出条件的行为。 有关详细信息，请参阅本主题后面部分中的[自定义修补程序配置文件](#BKMK_CONFIG_FILE)。

### <a name="configuration"></a>配置

配置以下设置。 有关详细信息，请参阅本主题中后面部分的链接。
- 包含要应用的更新以及修补程序配置文件的共享修补程序根文件夹的路径。 可以在 CAU UI 中键入此路径，或配置**HotfixRootFolderPath @ no__t-1 @ no__t-2Path >** PowerShell a @ no__t-.3in 参数。 

   > [!NOTE]
   > 可以将修补程序根文件夹指定为本地文件夹路径，或指定为 *\\ @ no__t-2ServerName @ no__t-3Share @ no__t-4RootFolderName*格式的 UNC 路径。 可以使用域 @ no__t-0based 或独立 DFS 命名空间路径。 但是，在修补程序配置文件中检查访问权限的即插即用的0in 功能与 DFS 命名空间路径不兼容，因此，如果您配置一个 DFS 命名空间路径，则必须使用 CAU UI 禁用对访问权限的检查，或者配置**DisableAclChecks @ no__t-2'True '** a @ no__t-.3in 参数。
- 承载修补程序根文件夹的服务器上的设置，用于检查访问该文件夹的适当权限，并确保从 SMB 共享文件夹中访问的数据的完整性 @no__t 0SMB 签名或 SMB 加密 @ no__t-1。 有关详细信息，请参阅 [限制对修补程序根文件夹的访问权限](#BKMK_ACL)。

### <a name="additional-options"></a>其他选项

- （可选）配置插件 @ no__t-0in，以便在从修补程序文件共享访问数据时强制执行 SMB 加密。 在 CAU UI 中的 "**其他选项**" 页上，选择 "**访问修补程序根文件夹时要求 SMB 加密**" 选项，或配置**RequireSMBEncryption @ no__t-3'True '** PowerShell a @ no__t-4in 参数。 
  > [!IMPORTANT]
  > 若要借助 SMB 签名或 SMB 加密启用 SMB 数据的完整性，则必须在 SMB 服务器上执行额外的配置步骤。 有关详细信息，请参阅[限制对修补程序根文件夹的访问权限](#BKMK_ACL)中的第 4 步。 如果你选择强制使用 SMB 加密选项，并且修补程序根文件夹未配置为使用 SMB 加密进行访问，则更新运行将失败。
- 或者，禁用针对修补程序根文件夹的足够权限和修补程序配置文件的默认检查。 在 CAU UI 中，选择 "**禁用对修补程序根文件夹和配置文件的管理员访问权限**"，或配置**DisableAclChecks @ no__t-2'True '** a @ no__t-.3in 参数。
- （可选）配置**HotfixInstallerTimeoutMinutes @ no__t-1 @ no__t-2**参数以指定修补程序插件 @ no__t-.3in 等待修补程序安装程序进程返回的时间。 @no__t 0The 默认值为30分钟。 \)例如，若要指定两个小时的超时时间，请设置**HotfixInstallerTimeoutMinutes @ no__t-1120**。
- （可选）配置**HotfixConfigFileName \= <name>** a @ .3in 参数以指定位于修补程序根文件夹中的修补程序配置文件的名称。 如果未指定，则使用默认名称 DefaultHotfixConfig.xml。
  
### <a name="BKMK_HF_ROOT"></a>配置修补程序根文件夹结构

要使修补程序插件 no__t-0in 工作，修补程序必须存储在 SMB 文件共享 \(hotfix 根文件夹 @ no__t）中的良好 @ no__t-1defined 结构中，并且必须使用 CAU UI 配置修补程序插件 @ no__t-4in，并使用修补程序根文件夹的路径CAU PowerShell cmdlet。 此路径将作为**HotfixRootFolderPath**参数传递到插件 @ no__t-0in。 如以下示例所示，根据更新需要，你可以为修补程序根文件夹选择多个结构中的一个。 不符合该结构的文件或文件夹将会忽略。  
  
#### <a name="example-1---folder-structure-used-to-apply-hotfixes-to-all-cluster-nodes"></a>示例 1-用于将修补程序应用到所有群集节点的文件夹结构
  
若要指定修补程序应用于所有群集节点，请将它们复制到修补程序根文件夹下名为**CAUHotfix @ no__t-1All**的文件夹。 在此示例中， **HotfixRootFolderPath**插件 @ no__t-1in 参数设置为 *\\ @ no__t-4MyFileServer @ no__t-5Hotfixes @ no__t-6Root @ no__t*。 **CAUHotfix @ no__t-1All**文件夹包含三个更新，它们将应用于所有群集节点，扩展名为 .msu、.msi 和 .msp。 更新文件名称仅用于说明目的。  
  
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
  
#### <a name="example-2---folder-structure-used-to-apply-certain-updates-only-to-a-specific-node"></a>示例 2-用于将某些更新仅应用于特定节点的文件夹结构
  
若要指定仅适用于特定节点的修补程序，请在具有节点名称的修补程序根文件夹下使用子文件夹。 使用群集节点的 NetBIOS 名称，例如，*ContosoNode1*。 然后，将仅适用于此节点的更新移动到该子文件夹。 在下面的示例中， **HotfixRootFolderPath**插件 @ no__t-1in 参数设置为 *\\ @ no__t-4MyFileServer @ no__t-5Hotfixes @ no__t-6Root @ no__t-7*。 **CAUHotfix @ no__t-1All**文件夹中的更新将应用到所有群集节点，*节点 1 @ no__t-3Specific\_Update.msu*将仅应用于*ContosoNode1*。  
  
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
  
#### <a name="example-3---folder-structure-used-to-apply-updates-other-than-msu-msi-and-msp-files"></a>示例 3-用于应用 .msu、.msi 和 .msp 文件以外更新的文件夹结构
  
默认情况下，**Microsoft.HotfixPlugin** 仅将应用具有 .msu、.msi 或 .msp 扩展名的更新。 但是，某些更新可能具有不同的扩展名，并且需要不同的安装命令。 例如，你可能需要将具有 .exe 扩展名的固件更新应用于群集中的节点。 你可以使用子文件夹配置修补程序根文件夹，该子文件夹指示应安装的特定非 @ no__t 更新类型。 你还必须配置相应的文件夹安装规则，该规则用于指定修补程序配置 XML 文件中的 `<FolderRules>` 元素内的安装命令。  
  
在下面的示例中， **HotfixRootFolderPath**插件 @ no__t-1in 参数设置为 *\\ @ no__t-4MyFileServer @ no__t-5Hotfixes @ no__t-6Root @ no__t-7*。 多个更新将应用于所有群集节点，并且通过使用 *FolderRule1* 将固件更新 *SpecialHotfix1.exe* 应用于 *ContosoNode1*。 有关在修补程序配置文件中配置 *ContosoNode1* 的详细信息，请参阅本主题后面部分中的 [自定义修补程序配置文件](#BKMK_CONFIG_FILE) 。  
  
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
  
**% systemroot% \\System32 @ no__t-2WindowsPowerShell @ no__t-3v 1.0 @ no__t-4Modules @ no__t-5ClusterAwareUpdating 文件夹**  
  
若要自定义修补程序配置文件，将从该位置的示例配置文件 DefaultHotfixConfig.xml 复制到修补程序根文件夹并为你的方案进行适当的修改。  
  
> [!IMPORTANT]  
> 若要应用由 Microsoft 和其他更新所提供的大部分修补程序，可以使用默认修补程序配置文件而无需任何修改。 自定义修补程序配置文件是仅在高级使用方案中才出现的任务。  
  
默认情况下，修补程序配置 XML 文件定义安装规则和针对以下两个修补程序类别的退出条件：  
  
-   带有扩展名的修补程序文件，可以默认 \(、.msi 和 .msp 文件 @ no__t 安装插件 @ no__t-0in。  
  
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
  
-   修补程序或其他不是 .msi、.msu 或 .msp 文件的更新文件，例如，非 @ no__t-0Microsoft 驱动程序、固件和 BIOS 更新。  
  
    每个非 @ no__t 0default 文件类型都配置为 `<FolderRules>` 元素中的 @no__t 1 元素。 `<Folder>` 元素的名称属性必须与将包含相应类型更新的修补程序根文件夹中文件夹的名称相同。 文件夹可以位于**CAUHotfix @ no__t-1All**文件夹中，也可以位于 node @ no__t-2specific 文件夹中。 例如，如果在修补程序根文件夹中配置 *FolderRule1* ，则在 XML 文件中配置以下元素以定义安装模板和该文件夹中更新的退出条件：  
  
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
  
1.  使用即插即用插件标识用于更新运行的用户帐户 no__t-0in  
  
2.  在 SMB 文件服务器上必要的组中配置此用户帐户  
  
3.  配置权限以访问修补程序根文件夹  
  
4.  配置 SMB 数据完整性的设置  
  
5.  在 SMB 服务器上启用 Windows 防火墙规则  
  
#### <a name="step-1-identify-the-user-account-that-is-used-for-updating-runs-by-using-the-hotfix-plug-in"></a>步骤 1： 使用修补程序插件标识用于更新运行的用户帐户 no__t-0in
  
在执行使用**microsoft.hotfixplugin**的更新运行期间，cau 中用于检查安全设置的帐户取决于是否在远程 @ no__t-1updating 模式或自 @ no__t-2updating 模式下使用 cau，如下所示：  
  
-   **远程 @ no__t-1updating 模式**对群集具有管理权限的帐户，用于预览和应用更新。  
  
-   **Self @ no__t-1updating 模式**在 Active Directory 为 CAU 群集角色配置的虚拟计算机对象的名称。 这是为 CAU 群集角色在 Active Directory 中预留的虚拟计算机对象的名称或由 CAU 为群集角色生成的名称。 若要获取由 CAU 生成的名称，请运行**Get @ no__t-1CauClusterRole** CAU PowerShell cmdlet。 在输出中，**ResourceGroupName** 是生成的虚拟计算机对象帐户的名称。  
  
#### <a name="step-2-configure-this-user-account-in-the-necessary-groups-on-an-smb-file-server"></a>步骤 2： 在 SMB 文件服务器上必要的组中配置此用户帐户
  
> [!IMPORTANT]  
> 你必须在 SMB 服务器上作为本地管理员帐户添加用于更新运行的帐户。 如果因为你组织中的安全策略而不允许这样做，则通过以下过程在 SMB 服务器上使用必要的权限来配置此帐户。  
  
##### <a name="to-configure-a-user-account-on-the-smb-server"></a>在 SMB 服务器上配置用户帐户  
  
1.  将用于更新运行的帐户添加到 Distributed COM Users 组和以下任一组：Power User、服务器操作或打印操作员。  
  
2.  若要启用帐户所需的 WMI 权限，请在 SMB 服务器上启动 WMI 管理控制台。 启动 PowerShell，然后键入以下命令：  
  
    ```  
    wmimgmt.msc  
    ```  
  
3.  在控制台树中，右键 @ no__t-0click **WMI Control \(Local @ no__t**，然后单击 "**属性**"。  
  
4.  单击“安全”，然后展开“根”。  
  
5.  单击“CIMV2”，然后单击“安全”。  
  
6.  将用于更新运行的帐户添加到“组或用户名”列表。  
  
7.  将“执行方法” 和“远程启用” 权限授予用于更新运行的帐户。  
  
#### <a name="step-3-configure-permissions-to-access-the-hotfix-root-folder"></a>步骤 3： 配置权限以访问修补程序根文件夹
  
默认情况下，当你尝试应用更新时，修补程序插件 @ no__t-0in 会检查 NTFS 文件系统权限的配置，以访问修补程序根文件夹。 如果文件夹访问权限配置不正确，则使用修补程序 no__t-0in 的更新运行可能会失败。  
  
如果使用修补程序插件的默认配置 no__t-0in，请确保文件夹访问权限满足以下要求。  
  
-   用户组具有读取权限。  
  
-   如果即插 @ no__t-0in 将使用 .exe 扩展名应用更新，则用户组具有 Execute 权限。  
  
-   仅允许某些安全主体 @no__t-不需要 @ no__t-0but 具有写入或修改权限。 允许的主体是本地管理员组、SYSTEM、CREATOR OWNER 和 TrustedInstaller。 其他帐户或组不允许具有写入或修改修补程序根文件夹的权限。  
  
（可选）可以禁用即插即用 no__t-0in 默认执行的前面的检查。 可以通过两种方式之一完成此操作：  
  
-   如果你使用的是 CAU PowerShell cmdlet，请在修补程序插件 @ no__t-.3in 的**CauPluginArguments**参数中配置**DisableAclChecks @ no__t-1'True '** 参数。  
  
-   如果使用 CAU UI，请选择用于配置更新运行选项的向导中“其他更新选项”页面上的“禁用对修补程序根文件夹和配置文件管理员访问权限的检查”选项。  
  
但是，作为在许多环境中的最佳实践，我们建议你使用默认的配置强制执行这些检查。  
  
#### <a name="step-4-configure-settings-for-smb-data-integrity"></a>步骤 4： 配置 SMB 数据完整性的设置
  
若要检查群集节点和 SMB 文件共享之间的连接中的数据完整性，修补程序插件 no__t-0in 要求你为 smb 签名或 SMB 加密启用 SMB 文件共享上的设置。 SMB 加密在许多环境中提供了增强的安全性和更好的性能，从 Windows Server 2012 开始受支持。 你可以启用其中一种设置或同时启用这两种设置，如下所示：  
  
-   若要启用 SMB 签名，请参阅 Microsoft 知识库内的 [文章 887429](https://support.microsoft.com/kb/887429) 中描述的过程。  
  
-   若要为 SMB 共享文件夹启用 SMB 加密，请在 SMB 服务器上运行以下 PowerShell cmdlet：  
  
    ```PowerShell  
    Set-SmbShare <ShareName> -EncryptData $true  
    ```  
  
    其中 <*ShareName*> 是 SMB 共享文件夹的名称。  
  
或者，若要在 SMB 服务器的连接中强制使用 SMB 加密，请选择 CAU UI 中的 "**访问修补程序根文件夹时要求 Smb 加密**" 选项，或配置**RequireSMBEncryption @ no__t-2'True '** a @ no__使用 CAU PowerShell cmdlet 的 .3in 参数。  
  
> [!IMPORTANT]  
> 如果你选择强制使用 SMB 加密的选项，并且修补程序根文件夹未配置为使用 SMB 加密的连接，则更新运行将失败。  
  
#### <a name="step-5-enable-a-windows-firewall-rule-on-the-smb-server"></a>步骤 5： 在 SMB 服务器上启用 Windows 防火墙规则
  
你必须在 SMB 文件服务器上的 Windows 防火墙中启用**文件服务器远程管理 \(SMB @ no__t-2in @ no__t**规则。 默认情况下，在 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 中启用此功能。  
  
## <a name="see-also"></a>请参阅  
  
-   [群集感知更新概述](cluster-aware-updating.md)
  
-   [群集感知更新 Windows PowerShell Cmdlet](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating)  
  
-   [群集感知更新插件参考](https://msdn.microsoft.com/library/hh418084.aspx)  
  
