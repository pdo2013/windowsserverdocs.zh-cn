---
ms.assetid: d44d4430-41e2-483a-9685-92610cdef32a
title: "群集的更新插件的工作原理"
ms.topic: article
ms.prod: windows-server-threshold
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 4/28/2017
ms.technology: storage-failover-clustering
description: "如何使用插件协调更新时使用群集的更新中的 Windows Server 群集上安装更新。"
ms.openlocfilehash: eb606dfbe6596ecabd35e6ac36624fab2b4436b9
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="how-cluster-aware-updating-plug-ins-work"></a>群集的更新插件的工作原理

>适用于：Windows Server（半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

[群集的更新](cluster-aware-updating.md)(CAU) 使用插件跨故障转移群集节点坐标安装更新。 本主题提供有关使用这些 built\ 中 CAU plug\ 接或 CAU 为你安装其他 plug\ 单元信息。

## <a name="BKMK_INSTALL"></a>安装 plug\  
默认 plug\ 接随 CAU 安装之外 plug\ 接 \ (**Microsoft.WindowsUpdatePlugin**和**Microsoft.HotfixPlugin**\) 必须单独安装。 如果 CAU 用于 self\ 更新模式中，在 plug\ 必须安装上所有群集节点。 如果 remote\ 更新的方式使用 CAU，则 plug\ 在必须安装更新处理协调器远程计算机上。 Plug\ 在你安装每个节点可能必须满足其他安装要求。  
  
若要安装 plug\ 中，请 plug\ 发布者按照说明进行操作。 若要手动注册 plug\ 中 CAU，运行[寄存器 CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin) cmdlet 了安装 plug\ 在每台计算机上的。  
  
## <a name="specify-a-plug-in-and-plug-in-arguments"></a>指定的 plug\ 中和 plug\ 在参数  
  
### <a name="specify-a-cau-plug-in"></a>指定 CAU plug\ 中

在 CAU UI 中，您 plug\ 中从列表中选择 drop\ 减小的可用 plug\ 接当你使用 CAU 执行以下操作：  
  
-   适用于群集的更新  
  
-   群集的的 preview 更新  
  
-   配置群集 self\ 更新选项  
  
默认情况下，选择在 plug\ CAU **Microsoft.WindowsUpdatePlugin**。 但是，你可以指定任何 plug\ 在的安装和使用 CAU 注册。

> [!TIP]  
> 在 CAU UI 中，你只能指定 plug\ 项以 CAU 使用预览或才能将更新应用在更新运行一次。 通过使用 CAU PowerShell cmdlet，你可以指定一个或多个 plug\ 接程序。如果你需要安装群集上多种类型的更新，它是一个更新运行时，在指定多个 plug\ 接通常更有效而不是使用单独的每个更新运行 plug\ 中。 例如，通常会发生较少节点重启。

通过使用下表列出了这些 CAU PowerShell cmdlet，你可以指定一个或多个 plug\ 接更新运行或扫描通过**– CauPluginName**参数。 你可以用逗号分隔指定多个 plug\ 中名称。 指定多个 plug\ 接，如果你还可以控制如何 plug\ 接彼此影响更新运行期间按指定**\-RunPluginsSerially**，**\-StopOnPluginFailure**，并**– SeparateReboots**参数。 有关使用多个 plug\ 接的详细信息，请使用到 cmdlet 文档，如下表中所提供的链接。  
  
|Cmdlet|描述|  
|----------|---------------|  
|[Add-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/add-cauclusterrole)|添加了 CAU 聚集的角色提供指定的群集 self\ 更新功能。|  
|[Invoke-CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-caurun)|适用的更新中执行的群集节点扫描并安装更新运行指定群集上通过这些更新。|  
|[Invoke-CauScan](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-causcan)|适用的更新中执行的群集节点扫描并返回的更新将应用于每个中指定的群集节点初始设置列表。|  
|[Set-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/set-cauclusterrole)|指定群集上设置为 CAU 聚集角色配置属性。|  
  
如果你不使用这些 cmdlet 指定 CAU plug\ 中参数，默认是 plug\ 在**Microsoft.WindowsUpdatePlugin**。  
  
### <a name="specify-cau-plug-in-arguments"></a>指定 CAU plug\ 中参数  
当您将配置更新运行选项时，你可以指定一项或多*name\ = 值*配对 \(arguments\) 所选 plug\ 中使用。 例如，在 CAU UI 中，你可以指定多个参数，如下所示：  
  
**Name1\ = 值 1;Name2\ = 值 2;Name3\ = Value3**  
  
这些*name\ = 值*对必须对该 plug\ 有意义指定。 对于某些 plug\ 接参数均是可选的。  
  
参数 plug\ 中 CAU 语法遵循以下常规规则：  
  
-   多个*name\ = 值*对分号的分隔。  
  
-   包含空格的值四面环引号，，例如：**Name1\ ="带有空格的值"**。  
  
-   确切语法*值*取决于 plug\ 中。  
  
若要指定 plug\ 中参数使用支持 CAU PowerShell cmdlet **– CauPluginParameters**参数，一种形式的参数 pass:  
  
**\-CauPluginArguments @{Name1\ = 值 1;Name2\ = 值 2;Name3\ = Value3}**  
  
你还可以使用预定义的 PowerShell 希表。 若要指定多个 plug\ 中的参数 plug\ 中通过分隔用逗号的参数多个希的表。 通过中指定的 plug\ 顺序 plug\ 中参数**CauPluginName**。  
  
### <a name="specify-optional-plug-in-arguments"></a>指定可选 plug\ 中参数  
Plug\ 接 CAU 安装 \ (**Microsoft.WindowsUpdatePlugin**和**Microsoft.HotfixPlugin**\) 提供更多选项，你可以选择。 在 CAU UI 中，其中显示在**其他选项**页后您将配置更新运行 plug\ 接选项。 如果你使用的 CAU PowerShell cmdlet，作为可选 plug\ 中参数配置这些选项。 有关详细信息，请参阅[使用 Microsoft.WindowsUpdatePlugin](#BKMK_WUP)和[使用 Microsoft.HotfixPlugin](#BKMK_HFP)本主题后面。  
  
## <a name="manage-plug-ins-using-windows-powershell-cmdlets"></a>管理 plug\ 接使用 Windows PowerShell cmdlet  
  
|Cmdlet|描述|  
|----------|---------------|  
|[Get-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/get-cauplugin)|检索一个或多个更新在本地计算机已注册的 plug\ 单元的软件的相关信息。|  
|[Register-CauPlugin]((https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin))|注册 CAU 软件更新在本地计算机上的 plug\。|  
|[Unregister-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/unregister-cauplugin)|会删除在从列表中可供 CAU 的 plug\ 单元的 plug\ 更新的软件。 **注意：** plug\ 接与 CAU 安装 \ (**Microsoft.WindowsUpdatePlugin**和**Microsoft.HotfixPlugin**\) 不能注销。|  
  
## <a name="BKMK_WUP"></a>使用 Microsoft.WindowsUpdatePlugin  

对于 CAU 的 plug\ 中的默认**Microsoft.WindowsUpdatePlugin**，执行以下操作：
- 与每个故障转移群集节点上的 Windows 更新代理，将需要为每个节点运行的 Microsoft 产品的更新应用的通信方式。
- 直接从 Windows 更新或 Microsoft 更新或 on\ 部署 Windows Server Update Services \(WSUS\) 服务器安装群集更新。
- 选中仅安装常规 distribution 发布 \(GDR\) 更新。 默认情况下，在 plug\ 适用仅重要的软件更新。 不不需要的任何配置。 默认配置下载并安装每个节点 GDR 的重要更新。 

> [!NOTE]
> 若要将默认选择最重要的软件更新以外的更新应用 \（例如，驱动程序 updates\），您可以配置可选 plug\ 参数。 有关详细信息，请参阅[配置 Windows 更新代理查询字符串](#BKMK_QUERY)。

### <a name="requirements"></a>要求

- 故障转移群集和远程更新处理协调器计算机 \(if used\) 必须满足对于 CAU 要求和中列出，才能进行远程管理配置[要求和最佳做法 CAU](cluster-aware-updating-requirements.md)。
- 查看[对 Microsoft 更新的应用建议](cluster-aware-updating-requirements.md#BKMK_BP_WUA)，然后为 Microsoft 更新配置故障转移群集节点进行任何需要的更改。
- 为了获得最佳效果，我们建议你运行的 CAU 最佳实践分析 \(BPA\) 要确保正确配置群集和更新环境使用 CAU 应用更新。 有关详细信息，请参阅[更新是否准备好测试 CAU](cluster-aware-updating-requirements.md#BKMK_BPA)。

> [!NOTE]
> 不需要 Microsoft 许可条款的接受或需要用户交互的更新范围内，并且必须手动安装。

### <a name="additional-options"></a>其他选项

（可选），你可以指定以下 plug\ 中参数扩充或限制的一套通过 plug\ 中应用的更新：
- 若要配置 plug\ 的应用推荐的更新，除了在每个节点 CAU UI 中，在重要更新到**其他选项**页上，选择**提供推荐更新接收重要更新的方式相同**复选框。
<br>或者，将配置**'IncludeRecommendedUpdates \ = 真'** plug\ 中参数。
- 若要配置 plug\ 接筛选 GDR 更新适用于每个群集节点类型，使用 Windows 更新代理查询字符串指定**查询字符串**plug\ 中参数。 有关详细信息，请参阅[配置 Windows 更新代理查询字符串](#BKMK_QUERY)。

### <a name="BKMK_QUERY"></a>配置 Windows 更新代理查询字符串  
您可以配置 plug\ 中的默认值的 plug\ 中参数**Microsoft.WindowsUpdatePlugin**，其中包含 Windows 更新代理 \(WUA\) 查询字符串。 此指令使用 WUA API 识别的 Microsoft 更新适用于每个节点，基于特定选择条件一个或多个组。 你可以通过使用逻辑和或逻辑或组合多个条件。 WUA 查询字符串指定 plug\ 参数中方式如下：  
  
**QueryString\ ="Criterion1\ = 值 1 和之间 / 或 Criterion2\ = 值 2 和之间 / 或…"**  
  
例如，**Microsoft.WindowsUpdatePlugin**自动选择使用默认值重要更新**查询字符串**使用构造的参数**IsInstalled**，**类型**，**IsHidden**，并**IsAssigned**条件：  
  
**QueryString\ ="IsInstalled\ = 0 和 Type\ = 软件和 IsHidden\ = 0 和 IsAssigned\ = 1"**  
  
如果你要指定**查询字符串**参数，使用它来替代默认**查询字符串**为 plug\ 在配置。  
  
#### <a name="example-1"></a>示例 1
  
若要配置**查询字符串**安装特定更新，具体由 ID 的参数*f6ce46c1\-971c\-43f9\-a2aa\-783df125f003*:  
  
**QueryString\ ="UpdateID\ ='f6ce46c1\-971c\-43f9\-a2aa\-783df125f003' 和 IsInstalled\ = 0"**  
  
> [!NOTE]  
> 上面的示例是有效的应用更新，通过 Cluster\ 感知更新向导。 如果你想要通过配置 self\ 更新 CAU ui 的选项或者使用安装特定更新**Add\ CauClusterRole**或**Set\ CauClusterRole**PowerShell cmdlet 必须格式化具有两个 single\ 报价字符 UpdateID 值：  
>   
> **QueryString\ ="UpdateID\ = f6ce46c1\-971c\-43f9\-a2aa\-783df125f003 ' 和 IsInstalled\ = 0"**  
  
#### <a name="example-2"></a>示例 2
  
若要配置**查询字符串**安装仅驱动程序的参数：  
  
**QueryString\ ="IsInstalled\ = 0 和 Type\ = 驱动程序和 IsHidden\ = 0"**  
  
有关查询字符串 plug\ 中的默认值的详细信息**Microsoft.WindowsUpdatePlugin**，搜索条件 \ (如**IsInstalled**\)，并可以包含在查询的字符串，语法查看有关中的搜索条件部分[Windows 更新代理 (WUA) API 参考](http://go.microsoft.com/fwlink/p/?LinkId=223304)。  
  
## <a name="BKMK_HFP"></a>使用 Microsoft.HotfixPlugin  
在 plug\ **Microsoft.HotfixPlugin**可用于应用 Microsoft 有限 distribution 发布 \(LDR\) 更新 \（也称为修补程序和以前称为 QFEs\），你下载独立若要解决 Microsoft 软件的特定问题。 插件从根文件夹 SMB 文件共享上安装的更新，也可以将自定义应用 non\ Microsoft 驱动程序、固件和 BIOS 更新。

> [!NOTE]
> 修补程序有时会从 Microsoft 知识库文章中的下载可用，但它们也将提供给客户 as\ 需要。

### <a name="requirements"></a>要求

- 故障转移群集和远程更新处理协调器计算机 \(if used\) 必须满足对于 CAU 要求和中列出，才能进行远程管理配置[要求和最佳做法 CAU](cluster-aware-updating-requirements.md)。
- 查看[使用 Microsoft.HotfixPlugin 建议](cluster-aware-updating-requirements.md#BKMK_BP_HF)。
- 为了获得最佳效果，我们建议你运行的 CAU 最佳实践分析 \(BPA\) 模型，以确保正确配置群集和更新环境使用 CAU 应用更新。 有关详细信息，请参阅[更新是否准备好测试 CAU](cluster-aware-updating-requirements.md#BKMK_BPA)。
- 从发布者，获取更新并将它们复制或服务器消息块 \(SMB\) 文件共享到中提取 \ (修补程序根 folder\) 的支持至少 SMB 2.0 和程序都可以访问的群集节点并更新处理协调器远程计算机的所有 \（如果 remote\ 更新 mode\ 用于 CAU）。 有关详细信息，请参阅[配置修补程序根文件夹结构](#BKMK_HF_ROOT)本主题后面。 

    > [!NOTE]
    > 默认情况下，此 plug\ 接仅安装修补程序与以下文件扩展名：.msu、.msi，以及.msp。

- 复制该 DefaultHotfixConfig.xml 文件 \ (中提供的**%systemroot%\\System32\\WindowsPowerShell\\v1.0\\Modules\\ClusterAwareUpdating**计算机上的文件夹所在 CAU 工具 installed\) 你为你创建的修补程序根文件夹和依据提取修补程序。 例如，将复制的配置文件*\\\MyFileServer\\Hotfixes\\Root\\*。 

    > [!NOTE]
    > 若要安装的 Microsoft 和其他更新提供的大多数修补程序，无需修改可以使用默认修补程序的配置文件。 如果你项 scenario 要求执行此操作，你可以自作为高级任务的配置文件。 配置文件包含自定义规则，例如，要处理具有特定的扩展修补程序文件，或者若要为特定退出条件定义的行为。 有关详细信息，请参阅[自定义的修补程序的配置文件](#BKMK_CONFIG_FILE)本主题后面。

### <a name="configuration"></a>配置

将以下设置配置。 详细信息，请参阅本主题中的各部分的链接。
- 共享修补程序根文件夹，其中包含更新将应用和程序路径包含修补程序的配置文件。 你可以键入 CAU UI 中的路径或配置**HotfixRootFolderPath\ = \ < 路径 >** PowerShell plug\ 中参数。 

   > [!NOTE]
   > 您可以指定的修补程序根文件夹作为一个本地文件夹的路径或窗体 unc *\\\ServerName\\Share\\RootFolderName*。 可以使用 domain\ 基于或独立 DFS Namespace 路径。 但是，检查修补程序的配置文件中的访问权限与不兼容的 DFS Namespace 路径，所以如果配置之一，你必须禁用检查 plug\ 功能访问权限，通过使用 CAU UI 或配置**DisableAclChecks\ = '真**plug\ 中参数。
- 设置承载修补程序根文件夹，若要检查相应的权限来访问该文件夹，并确保通过 SMB 访问数据的完整性服务器上的共享文件夹 \ (SMB 登录或 SMB Encryption\)。 有关详细信息，请参阅[限制对修补程序根文件夹的访问](#BKMK_ACL)。

### <a name="additional-options"></a>其他选项

- （可选）配置 plug\ 中，以便访问来自该修补程序的文件共享的数据时，实施 SMB 加密。 在 CAU UI 中，在**其他选项**页上，选择**访问修补程序根文件夹中的需要 SMB 加密**选项，或配置**RequireSMBEncryption\ = '真**PowerShell plug\ 中参数。 
  > [!IMPORTANT]
  > 你必须执行上 SMB 服务器以启用 SMB SMB 登录或 SMB 加密的数据完整性的其他配置步骤。 有关详细信息，请参阅在第 4 步[限制对修补程序根文件夹的访问](#BKMK_ACL)。 如果你选择的选项实施使用 SMB 加密和修补程序根文件夹未配置访问使用 SMB 加密，将失败，更新运行。
- （可选）禁用足够权限的修补程序根文件夹和修补程序的配置文件的默认检查。 在 CAU UI 中，选择**禁用的管理员修补程序根文件夹和配置文件的访问权限的检查**，或配置**DisableAclChecks\ = '真**plug\ 中参数。
- （可选）配置**HotfixInstallerTimeoutMinutes\ =<Integer>**参数若要指定的时长 plug\ 中的修复程序等待返回的修补程序 installer 过程。 \ (默认为 30 分钟。\) 例如，若要指定的两个小时时间超时，设置**HotfixInstallerTimeoutMinutes\ = 120**。
- （可选）配置**HotfixConfigFileName \ = <name>**plug\ 中参数指定位于修补程序根文件夹中的修补程序配置文件的名称。 如果未指定，则使用默认名称 DefaultHotfixConfig.xml。
  
### <a name="BKMK_HF_ROOT"></a>配置修补程序根文件夹结构

为了 plug\ 中的修复程序工作，修补程序必须将存储在 SMB 文件共享 well\ 定义结构 \ (修补程序根 folder\)，必须使用 CAU UI 或 CAU PowerShell cmdlet 修补程序根文件夹的路径配置 plug\ 中的修复程序和。 此路径传递到为 plug\ 接**HotfixRootFolderPath**参数。 在以下示例所示，你可以根据您的更新需要选择几个修补程序根文件夹，结构之一。 将忽略的文件或文件夹不符合结构。  
  
#### <a name="example-1---folder-structure-used-to-apply-hotfixes-to-all-cluster-nodes"></a>使用适用于所有群集节点的修补程序的示例 1-文件夹结构
  
若要指定，修补程序适用于所有群集节点，请将它们复制到一个名为文件夹**CAUHotfix\_All**修补程序根文件夹下。 在此示例中，**HotfixRootFolderPath**参数 plug\ 中设置为*\\\MyFileServer\\Hotfixes\\Root\\*。 **CAUHotfix\_All**文件夹包含三个更新的扩展.msu、.msi，和.msp 将应用于所有群集节点。 仅用于图目的将更新文件的名称。  
  
> [!NOTE]  
> 在此和下面的示例中，在其所需位置修补程序根文件夹中显示为其默认名称 DefaultHotfixConfig.xml 修补程序配置文件。  
  
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
  
若要指定仅适用于特定节点的修补程序，请使用节点同名的修补程序根文件夹下的子文件夹。 例如，使用群集节点上的 NetBIOS 名称*ContosoNode1*。 然后，移动此节点向此子文件夹仅适用于的更新。 在以下示例中，**HotfixRootFolderPath**参数 plug\ 中设置为*\\\MyFileServer\\Hotfixes\\Root\\*。 在更新**CAUHotfix\_All**文件夹将应用于所有群集节点，然后*Node1\_Specific\_Update.msu*将仅可用于*ContosoNode1*。  
  
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
  
默认情况下，**Microsoft.HotfixPlugin**只适用用.msu、.msi 或.msp 扩展的更新。 但是，某些更新可能具有不同的扩展，并且需要安装其他命令。 例如，你可能需要应用中的群集节点向以扩展.exe 固件更新。 你可以使用指出特定的子文件夹配置修补程序根文件夹，应安装 non\ 默认更新类型。 您还必须配置指定中的安装命令相应文件夹安装规则`<FolderRules>`元素修补程序的配置 XML 文件中。  
  
在以下示例中，**HotfixRootFolderPath**参数 plug\ 中设置为*\\\MyFileServer\\Hotfixes\\Root\\*。 多个更新将应用于所有群集节点，然后固件更新*SpecialHotfix1.exe*将应用于*ContosoNode1*使用*FolderRule1*。 了解有关配置*FolderRule1*在修补程序的配置文件，请参阅[自定义的修补程序的配置文件](#BKMK_CONFIG_FILE)本主题后面。  
  
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

### <a name="BKMK_CONFIG_FILE"></a>自定义的修补程序的配置文件  
修补程序的配置文件的控制如何**Microsoft.HotfixPlugin**故障转移群集中安装修补程序的特定文件类型。 配置文件的 XML 方案中 HotfixConfigSchema.xsd，在装有 CAU 工具一台计算机上的以下文件夹位于定义：  
  
**%systemroot%\\System32\\WindowsPowerShell\\v1.0\\Modules\\ClusterAwareUpdating 文件夹**  
  
要自定义的修补程序的配置文件，并从该位置的样本配置文件 DefaultHotfixConfig.xml 复制到修补程序根文件夹相应修改您的方案。  
  
> [!IMPORTANT]  
> 应用提供的 Microsoft 和其他更新的大多数修补程序，无需修改可以使用默认修补程序的配置文件。 自定义的修补程序的配置文件是仅在高级的使用情况的任务。  
  
默认情况下，修补程序的配置 XML 文件定义安装规则和退出条件以下两个类别的修补程序：  
  
-   默认情况下可以安装 plug\ 中的扩展修补程序文件 \ (.msu、.msi，以及.msp files\)。  
  
    以下指`<ExtensionRules>`中的元素`<DefaultRules>`元素。 目前还有一个需要`<Extension>`元素为每台默认支持的文件类型。 常规 XML 结构如下所示：  
  
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
  
    如果你需要将某些更新类型应用于所有您的环境中的群集节点，你可以定义其他`<Extension>`元素。  
  
-   修复程序或其他更新文件不是.msi、.msu 或.msp 文件，例如，non\ Microsoft 驱动程序、固件和 BIOS 更新。  
  
    每个 non\ 默认文件类型配置为`<Folder>`中的元素`<FolderRules>`元素。 名称特性`<Folder>`元素必须相同的修补程序根文件夹将包含相应类型的更新中的文件夹的名称。 该文件夹可以在**CAUHotfix\_All**文件夹或 node\ 特定文件夹中。 例如，如果*FolderRule1*是配置的修补程序根文件夹中，将配置 XML 文件定义安装模板和退出条件该文件夹中的更新中的以下元素：  
  
    ```xml  
    <FolderRules>  
          <Folder name="FolderRule1">  
            <!-- Template and ExitConditions elements for installation of updates in FolderRule1 follow -->  
             ...  
          </Folder>  
          ...  
    </FolderRules>  
    ```  
  
下表介绍了`<Template>`特性和可能`<ExitConditions>`子。  
  
|`<Template>` 属性|描述|  
|--------------------------|---------------|  
|`path`|中定义的文件类型的安装程序的完整路径`<Extension name>`属性。<br /><br />若要指定中的修补程序根文件夹结构更新文件路径，请使用`$update$`。|  
|`parameters`|需要和可选参数中指定的程序的字符串`path`。<br /><br />若要指定的更新中的修补程序根文件夹结构文件路径参数，可使用`$update$`。|  
  
|`<ExitConditions>` 子|描述|  
|---------------------------------|---------------|  
|`<Success>`|定义指示成功指定的更新的一个或多个退出代码。 这是需要的子。|  
|`<Success_RebootRequired>`|（可选）定义一个或多个退出代码来指示成功指定的更新和节点必须重新启动。 <br>**注意：**（可选）`<Folder>`元素可以包含`alwaysReboot`属性。 如果此特性设置，它表示如果通过此规则中安装修补程序返回一个中定义了退出代码`<Success>`，它被视为`<Success_RebootRequired>`退出条件。|  
|`<Fail_RebootRequired>`|（可选）定义表示指定的更新失败并节点必须重新启动的一个或多个退出代码。|  
|`<AlreadyInstalled>`|（可选）定义指示指定的更新不应用，因为它已安装的一个或多个退出代码。|  
|`<NotApplicable>`|（可选）定义指示因为它不适用于群集节点未应用指定的更新的一个或多个退出代码。|  
  
> [!IMPORTANT]  
> 任何退出不显式中定义的代码`<ExitConditions>`解释更新失败，以及节点不重新启动。  
  
### <a name="BKMK_ACL"></a>限制对修补程序根文件夹的访问权限  
你必须执行配置 SMB 文件服务器和文件共享来帮助保护的修复程序根文件夹的文件和 hofix 仅在环境中访问配置文件的几个步骤**Microsoft.HotfixPlugin**。 这些步骤启用帮助防止可能篡改以可能损害故障转移群集一种的修补程序文件的多个功能。  
  
下面是常规步骤：  
  
1.  找出用于更新运行 plug\ 中使用的用户帐户  
  
2.  配置中必要的组 SMB 文件服务器上的此用户帐户  
  
3.  配置修补程序根文件夹的访问权限  
  
4.  配置 SMB 数据的完整性的设置  
  
5.  启用 SMB 服务器上的 Windows 防火墙规则  
  
#### <a name="step-1-identify-the-user-account-that-is-used-for-updating-runs-by-using-the-hotfix-plug-in"></a>第 1 步。 找出用于更新运行通过 plug\ 中的修复程序的用户帐户
  
使用 CAU 更新运行使用执行操作时检查安全设置该帐户**Microsoft.HotfixPlugin**取决于是否 CAU 使用在 remote\ 更新或 self\ 更新模式下，如下所示：  
  
-   **Remote\ 更新模式**具有管理员权限在群集预览和应用更新的帐户。  
  
-   **Self\ 更新模式**虚拟计算机对象配置了 Active Directory 中 CAU 名称群集且的角色。 这是预备虚拟的计算机中 Active Directory 对象 CAU 聚集角色名称或聚集角色 CAU 生成的名称。 如果它由 CAU 生成获得的名称，运行**Get\ CauClusterRole** CAU PowerShell cmdlet。 在输出中，**ResourceGroupName**是生成虚拟计算机对象帐户的名称。  
  
#### <a name="step-2-configure-this-user-account-in-the-necessary-groups-on-an-smb-file-server"></a>第 2 步。 配置中必要的组 SMB 文件服务器上的此用户帐户
  
> [!IMPORTANT]  
> 你必须添加的帐户，以用于更新运行上 SMB 服务器的本地管理员帐户。 如果这不允许由于你的组织中的安全策略，为配置此帐户所需的权限 SMB 服务器上通过以下步骤。  
  
##### <a name="to-configure-a-user-account-on-the-smb-server"></a>若要配置 SMB 服务器上的用户帐户  
  
1.  添加的帐户，用于更新运行到分布式 COM 用户组和一个以下部分：高级用户，服务器操作或打印运营商。  
  
2.  若要启用该帐户的必要 WMI 权限，启动 SMB 服务器 WMI 管理控制台。 开始 PowerShell，然后键入以下命令：  
  
    ```  
    wmimgmt.msc  
    ```  
  
3.  控制台树中 right\ 单击**WMI 控制 \(Local\)**，然后单击**属性**。  
  
4.  单击**安全**，然后展开**根**。  
  
5.  单击**CIMV2**，然后单击**安全**。  
  
6.  添加的帐户，以用于更新运行到**组或用户名**列表。  
  
7.  授予**执行方法**和**远程启用**用于更新运行该帐户的权限。  
  
#### <a name="step-3-configure-permissions-to-access-the-hotfix-root-folder"></a>第 3 步。 配置修补程序根文件夹的访问权限
  
默认情况下，当你尝试在应用更新，plug\ 中的修复程序将检查配置的修补程序根文件夹的访问权限 NTFS 文件系统权限。 如果未正确配置文件夹的访问权限，可能无法更新使用运行 plug\ 中的修复程序。  
  
如果你使用的默认配置 plug\ 中的修复程序，请确保文件夹的访问权限满足以下要求。  
  
-   用户组读取权限。  
  
-   如果在 plug\ 将应用使用.exe 扩展的更新，用户组具有执行权限。  
  
-   允许某些的安全主体 \（但不是 required\）已写或修改的权限。 允许的主体是的本地管理员组，系统、CREATOR 所有者和 TrustedInstaller。 其他帐户或组不允许有写入或修改的修补程序根文件夹中的权限。  
  
（可选），您可以禁用 plug\ 在默认情况下执行上述检查。 你可以在两种方式之一：  
  
-   如果你使用的 CAU PowerShell cmdlet、配置**DisableAclChecks\ = '真**中的参数**CauPluginArguments**参数 plug\ 中的修复程序。  
  
-   如果你使用的 CAU UI，请选择**禁用的管理员修补程序根文件夹和配置文件的访问权限的检查**选项卡上**更新的其他选项**用来配置更新运行选项向导中的页面。  
  
但是，许多环境中的最佳做法，我们建议你使用的默认配置强制这些检查。  
  
#### <a name="step-4-configure-settings-for-smb-data-integrity"></a>第 4 步。 配置 SMB 数据的完整性的设置
  
若要检查之间的群集节点并 SMB 文件共享连接中的数据完整性，plug\ 中的修复程序会要求你启用 SMB 文件共享 SMB 登录或 SMB 加密上的设置。 SMB 加密，不提供增强的安全性和许多环境中的更好的性能，支持在 Windows Server 2012 启动。 可以如下所示启用一项或两这些设置：  
  
-   若要启用 SMB 登录，请参阅中的过程[文章 887429](http://support.microsoft.com/kb/887429) Microsoft 知识库文章中。  
  
-   若要启用 SMB 加密 SMB 共享文件夹，请运行 SMB 服务器上的以下 PowerShell cmdlet:  
  
    ```PowerShell  
    Set-SmbShare <ShareName> -EncryptData $true  
    ```  
  
    其中 <*共享名*> 是 SMB 共享文件夹的名称。  
  
或者，若要强制中连接到 SMB 服务器 SMB 加密的使用，请选择**访问修补程序根文件夹中的需要 SMB 加密**选项在 CAU UI 中，或配置**RequireSMBEncryption\ = '真**使用 CAU PowerShell cmdlet plug\ 中的参数。  
  
> [!IMPORTANT]  
> 如果你选择的选项实施使用 SMB 加密和修补程序根文件夹未配置为使用 SMB 加密的连接，将失败，更新运行。  
  
#### <a name="step-5-enable-a-windows-firewall-rule-on-the-smb-server"></a>第 5 步。 启用 SMB 服务器上的 Windows 防火墙规则
  
你必须启用**文件服务器远程管理 \(SMB\-in\)**规则 Windows 防火墙在 SMB 文件服务器上的。 默认情况下在 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 被启用此选项。  
  
## <a name="see-also"></a>请参阅  
  
-   [群集的更新概述](cluster-aware-updating.md)
  
-   [群集的更新的 Windows PowerShell Cmdlet](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating)  
  
-   [群集更新插件参考](http://msdn.microsoft.com/library/hh418084.aspx)  
  
