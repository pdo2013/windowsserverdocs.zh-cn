---
ms.assetid: 2f4b6641-0ec2-4b1c-85fb-a1f1d16685c8
title: 群集感知更新高级选项和更新运行配置文件
ms.topic: article
ms.prod: windows-server-threshold
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 08/06/2018
description: 如何配置高级的选项和更新运行配置文件的群集感知更新 (CAU)
ms.openlocfilehash: 5fac31ad35422e623b98caaabdd9eae183e2e5d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830548"
---
# <a name="cluster-aware-updating-advanced-options-and-updating-run-profiles"></a>群集感知更新高级选项和更新运行配置文件

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

本主题介绍可以为配置的更新运行选项[群集感知更新](cluster-aware-updating.md)(CAU) 更新运行。 当你使用 CAU UI 或 CAU Windows PowerShell cmdlet 来应用更新或者配置自我更新选项时，可以配置这些高级的选项。

大多数配置设置可以另存为称为“更新运行配置文件”的 XML 文件，以后执行更新运行时可以重复使用该文件。 还可以在许多群集环境中使用 CAU 提供的更新运行选项的默认值。

你可以为每个更新运行指定其他选项，此方面的信息以及更新运行配置文件的信息，请参阅以下部分（本主题后面提供）：

指定当请求更新运行使用更新运行配置文件选项可以在更新运行配置文件中设置的选项

下表列出了你可以在 CAU 更新运行配置文件中设置的选项。 

> [!NOTE] 
> 若要设置 PreUpdateScript 或 PostUpdateScript 选项，请确保安装了 Windows PowerShell 和.NET Framework 4.6 或 4.5 和，在群集中每个节点上启用 PowerShell 远程处理。 有关详细信息，请参阅为中的远程管理配置节点[要求和群集感知更新的最佳做法](cluster-aware-updating-requirements.md)。


|Option|默认值|详细信息|  
|------------|-------------------|-------------|  
|**StopAfter**|无时间限制|时间（分钟），该时间过后将停止未完成的更新运行。 **注意：** 如果指定更新前或更新后 PowerShell 脚本，运行脚本以及执行更新的整个过程必须在完成内**StopAfter**时间限制。|  
|**WarnAfter**|默认情况下不显示警告|时间（分钟），如果更新运行（包括更新前脚本和更新后脚本（如果已配置））在该时间过后仍未完成，则将显示警告。|  
|**MaxRetriesPerNode**|3|每个节点将重试更新过程（包括更新前脚本和更新后脚本（如果已配置））的最大次数。 最大值为 64。|  
|**MaxFailedNodes**|对于大多数群集而言，它是一个大约为群集节点数三分之一的整数|更新可能会失败（原因是节点失败或者群集服务停止运行）的最大节点数。 如果多个节点失败，则停止更新运行。<br /><br /> 有效值范围为群集节点数减 0 到减 1。|  
|**RequireAllNodesOnline**|无|指定更新开始之前所有节点都必须联机且可以访问。|  
|**RebootTimeoutMinutes**|15|CAU 给重新启动节点（如果有必要重新启动）以及启动所有自动启动服务留出的时间（分钟）。 如果重新启动过程未在此时间内完成，该节点上更新运行标记为失败。|  
|**PreUpdateScript**|无|要更新开始之前，以及节点进入维护模式之前，每个节点上运行的 PowerShell 脚本路径和文件名称。 文件扩展名必须为 **.ps1**，并且路径加上文件的总长度不得超过 260 个字符。 最佳做法是，应该将脚本放置在群集存储中的磁盘上，或者放置在可用性高的网络文件共享上，以确保所有群集节点始终都可以对该脚本进行访问。 如果将脚本放置在网络文件共享上，请确保为 Everyone 组配置对该文件共享的读取权限，并限制写入访问权限，以防止未经授权的用户篡改文件。<br /><br /> 如果指定更新前脚本，请确保配置诸如时间限制之类的设置（例如“StopAfter”），以便让脚本成功运行。 这些限制跨运行脚本和安装更新的整个过程，而不仅仅是安装更新的过程。|  
|**PostUpdateScript**|无|要更新之后运行的 PowerShell 脚本的路径和文件名称完成 （节点退出维护模式后）。 文件扩展名必须为 **.ps1**并且路径加上文件的总长度不得超过 260 个字符。 最佳做法是，应该将脚本放置在群集存储中的磁盘上，或者放置在可用性高的网络文件共享上，以确保所有群集节点始终都可以对该脚本进行访问。 如果将脚本放置在网络文件共享上，请确保为 Everyone 组配置对该文件共享的读取权限，并限制写入访问权限，以防止未经授权的用户篡改文件。<br /><br /> 如果指定更新后脚本，则确保配置诸如时间限制之类的设置（例如“StopAfter”），以便让脚本成功运行。 这些限制跨运行脚本和安装更新的整个过程，而不仅仅是安装更新的过程。|  
|**ConfigurationName**|仅当运行脚本时此设置才有效。<br /><br /> 如果指定更新前脚本或更新后脚本，但不是指定**ConfigurationName**、 使用 PowerShell (Microsoft.PowerShell) 配置的默认会话。|指定定义会话中的脚本的 PowerShell 会话配置 (由指定**PreUpdateScript**并**PostUpdateScript**) 运行，且能够限制可运行的命令。|  
|**CauPluginName**|**Microsoft.WindowsUpdatePlugin**|配置群集感知更新以用于预览更新或执行更新运行的插件。 有关详细信息，请参阅[如何群集感知更新插件工作](cluster-aware-updating-plug-ins.md)。|  
|**CauPluginArguments**|无|针对要使用的更新插件的一组 *name=value* 对（参数），例如：<br /><br /> **Domain=Domain.local**<br /><br /> 这些 *name=value* 对必须对在“CauPluginName”中指定的插件有意义。<br /><br /> 若要使用 CAU UI 指定参数，请键入 *name*，按 Tab 键，然后键入相应的 *value*。 再次按 Tab 键以提供下一个参数。 每个 *name* 和 *value* 都用一个等号 (=) 自动分隔。 多个对用分号自动分隔。<br /><br /> 默认情况下**Microsoft.WindowsUpdatePlugin**插件，则不需要任何参数。 但是，可以指定一个可选参数，例如，指定一个标准的 Windows 更新代理查询字符串，用于筛选插件应用的更新集。 有关*名称*，使用**QueryString**，并为*值*，完整查询用引号引起来。<br /><br /> 有关详细信息，请参阅[如何群集感知更新插件工作](cluster-aware-updating-plug-ins.md)。|  
  
##  <a name="BKMK_runtime"></a> 指定当请求更新运行选项  
 下表列出了请求更新运行时可以指定的选项（不是更新运行配置文件中的选项）。 有关在更新运行配置文件中可以设置的选项的信息，请参阅上表。  
  
|Option|默认值|详细信息|  
|------------|-------------------|-------------|  
|**ClusterName**|无 <br>**注意：** 仅当未在故障转移群集节点上运行 CAU UI，或者你想要引用一个未运行 CAU UI 的故障转移群集时，才需要设置此选项。|要在其上执行更新运行的群集的 NetBIOS 名称。|  
|**凭据**|当前帐户凭据|将执行更新运行的目标群集的管理凭据。 您从可能已具有所需的凭据启动 CAU UI （或打开 PowerShell 会话中，如果你使用 CAU PowerShell cmdlet） 如果在群集具有管理员权利和权限的帐户。|  
|**NodeOrder**|默认情况下，CAU 从拥有群集角色数量最少的节点开始，然后是数量第二少的节点，依此类推。|群集节点的名称，顺序为应对它们进行更新的顺序（如果可能）。|  
  
##  <a name="BKMK_profile"></a> 使用更新运行配置文件  
 可将每个更新运行与特定的更新运行配置文件进行关联。 更新运行配置文件存储在默认 *%windir%\cluster*文件夹。 如果使用 CAU UI 中远程更新模式下，你可以在应用更新时指定更新运行配置文件，也可以使用的默认更新运行配置文件。 如果在自我更新模式使用 CAU，你可以从导入设置指定更新运行配置文件配置自我更新选项时。 在这两种情况下，都可以根据需要覆盖更新运行选项的显示值。 如果需要，可以使用相同或不同的文件名将更新运行选项另存为更新运行配置文件。 下一次当你应用更新或者配置自我更新选项时，CAU 将自动选择上次所选择的更新运行配置文件。  
  
 可以修改现有更新运行配置文件或通过选择创建新**创建或修改更新运行配置文件**CAU UI 中。

下面是有关使用更新运行配置文件的一些重要注意事项：

* 更新运行配置文件不存储特定于群集的管理凭据等信息。 如果你要在自我更新模式使用 CAU，更新运行配置文件也不存储自我更新计划信息。 这样便可在指定类的所有故障转移群集之间共享更新运行配置文件。
* 如果配置自我更新选项时使用更新运行配置文件和更高版本修改具有不同的值更新运行选项的配置文件，则自我更新配置不会自动更改。 若要应用新的更新运行设置，必须再次配置自我更新选项。
* 运行配置文件编辑器遗憾的是不支持包括空格，如下所述的文件路径*C:\Program Files*。 解决方法是，存储你之前和在不包含空格，或以独占方式使用 PowerShell 管理运行配置文件，将运行时放在路径两边的引号的路径中发布的更新脚本**Invoke-caurun**。

### <a name="windows-powershell-equivalent-commands"></a>Windows PowerShell 等效命令
  
 您可以从导入设置更新运行配置文件在运行时**Invoke-caurun**， **Add-cauclusterrole**，或**Set-cauclusterrole** cmdlet。  
  
 以下示例将使用 *C:\Windows\Cluster\DefaultParameters.xml* 中指定的更新运行选项，在名为 *CONTOSO-FC1* 的群集上执行扫描和完整的更新运行。 为剩余的 cmdlet 参数使用默认值。  
  
```powershell  
$MyRunProfile = Import-Clixml C:\Windows\Cluster\DefaultParameters.xml  
Invoke-CauRun –ClusterName CONTOSO-FC1 @MyRunProfile   
```  
  
 通过更新运行配置文件，你可以使用异常管理、时间限制和其他操作参数的一致性设置反复更新某个故障转移群集。 由于这些设置通常特定于某一类故障转移群集 - 例如“所有 Microsoft SQL Server 群集”或“我的业务关键型群集”- 你可能需要根据每个更新运行配置文件可以与之配合使用的故障转移群集种类来命名该配置文件。 此外，你可能需要在可由 IT 组织中特定类的所有故障转移群集访问的文件共享上管理更新运行配置文件。  
  
  
  
## <a name="see-also"></a>请参阅

-   [群集感知更新](cluster-aware-updating.md)
  
-   [在 Windows PowerShell 中的群集感知更新 Cmdlet](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps)