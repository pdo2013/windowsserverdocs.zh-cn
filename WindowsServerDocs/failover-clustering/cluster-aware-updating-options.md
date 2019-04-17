---
ms.assetid: 2f4b6641-0ec2-4b1c-85fb-a1f1d16685c8
title: "添彩注意到的高级选项的更新和更新运行配置文件"
ms.topic: article
ms.prod: storage-failover-clustering
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 6/7/2017
description: "如何配置高级的选项和更新为群集的更新 (CAU) 运行配置文件"
ms.openlocfilehash: 5b6f035791a946ff96ff6a95a1f753ef505d54b4
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="cluster-aware-updating-advanced-options-and-updating-run-profiles"></a>高级选项和更新运行配置文件的群集的更新

>适用于：Windows Server（半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主题介绍的可配置更新运行选项[群集的更新](cluster-aware-updating.md)更新运行 (CAU)。 当你使用 CAU UI 或 CAU Windows PowerShell cmdlet 才能将更新应用，或配置更新自行选项，可配置这些高级的选项。

可以保存大多数配置设置，XML 文件称为更新运行个人资料以及更高版本的更新运行重复使用。 由 CAU 运行更新选项的默认值还可在许多群集环境。

有关其他选项，你可以为每个更新运行指定以及更新运行配置文件的信息，请参阅本主题后面以下部分：

您可以指定时你请求更新运行使用更新运行配置文件选项可设置在更新运行个人资料的选项

下表列出了可设置在 CAU 更新运行个人资料的选项。 

> [!NOTE] 
> 若要设置的 PreUpdateScript 或 PostUpdateScript 选项，确保已安装了 Windows PowerShell 和.NET Framework 4.6 或 4.5 和每个中的群集节点上启用了 PowerShell 远程处理。 有关详细信息，请参阅配置远程管理节点[要求和最佳做法群集的更新](cluster-aware-updating-requirements.md)。


|选项|默认值|详细信息|  
|------------|-------------------|-------------|  
|**StopAfter**|无限制的时间|在几分钟内后，更新运行将停止如果它尚未完成的时间。 **注意：**运行脚本并执行更新的整个过程指定预更新或 PowerShell 脚本更新后，如果必须完成内**StopAfter**时间限制。|  
|**WarnAfter**|默认情况下，不会显示警告|之后，如果尚未完成更新（包括运行更新前脚本和更新后脚本，如果他们配置）将显示一个警告分钟时间。|  
|**MaxRetriesPerNode**|3|最大数目将每个节点重试更新过程（包括预更新脚本和更新后脚本，如果他们配置）的时间。 最大是 64。|  
|**MaxFailedNodes**|对于大多数群集，而是在大约 1-3 群集节点数表示|最大数目节点上的更新可能会失败，或者因为节点无法或群集服务停止运行。 如果一个多个节点失败，更新运行已停止。<br /><br /> 值的有效范围是从 0 到 1 小于群集节点数。|  
|**RequireAllNodesOnline**|无|指定所有节点必须都处于联机状态，并且在更新之前可以访问开始。|  
|**RebootTimeoutMinutes**|15|（如果需要重启）重启节点和开始所有自动启动的服务允许 CAU 分钟时间。 如果重启过程不在此时间内完成，更新运行该节点上标记为失败。|  
|**PreUpdateScript**|无|开始更新前，并且之前节点放入维护模式每个节点上运行 PowerShell 脚本路径和名称。 文件扩展名必须**.ps1**，并且总路径与文件的长度不能超过 260 的字符。 作为最佳做法，脚本应位于磁盘群集存储中或在高度可用网络文件共享，以确保它已始终可以访问所有群集节点。 当此脚本位于网络文件共享，请确保将配置文件共享的阅读许可的每个人都分组，并限制写入访问权限，以防止未经授权的用户文件篡改。<br /><br /> 如果指定脚本更新前，请确保如时间设置限制 (例如，**StopAfter**) 配置允许脚本成功地运行。 这些限制跨整个运行脚本，安装更新，而不仅仅是安装更新的进程的进程。|  
|**PostUpdateScript**|无|（节点离开维护模式）后，更新完成后运行 PowerShell 脚本路径和名称。 文件扩展名必须**.ps1**和总路径与文件的长度不能超过 260 的字符。 作为最佳做法，脚本应位于磁盘群集存储中或在高度可用网络文件共享，以确保它已始终可以访问所有群集节点。 当此脚本位于网络文件共享，请确保将配置文件共享的阅读许可的每个人都分组，并限制写入访问权限，以防止未经授权的用户文件篡改。<br /><br /> 如果指定脚本更新后，请确保如时间设置限制 (例如，**StopAfter**) 配置允许脚本成功地运行。 这些限制跨整个运行脚本，安装更新，而不仅仅是安装更新的进程的进程。|  
|**配置名**|此设置仅起作用，如果你运行的脚本。<br /><br /> 如果你指定更新前脚本或更新后脚本，但不是指定**配置名**，PowerShell 默认会话配置 (Microsoft。PowerShell) 使用。|指定定义会话中的脚本 PowerShell 会话配置 (由**PreUpdateScript**和**PostUpdateScript**) 运行，并且可以限制可以运行命令。|  
|**CauPluginName**|**Microsoft.WindowsUpdatePlugin**|插件，您将配置群集的更新可用于预览更新或执行更新运行。 有关详细信息，请参阅[如何群集的更新插件工作](cluster-aware-updating-plug-ins.md)。|  
|**CauPluginArguments**|无|一套*名称 = 值*对（参数）的更新的插件，若要使用，例如：<br /><br /> **Domain=Domain.local**<br /><br /> 这些*名称 = 值*对必须对该插件有意义中指定的**CauPluginName**。<br /><br /> 若要指定参数使用 CAU UI，键入*名称*，按下 Tab 键，然后键入相应*值*。 按 Tab 键再次向下一个参数。 每个*名称*和*值*自动等于号（=）的分隔。 多个对自动分号的分隔。<br /><br /> 对于默认**Microsoft.WindowsUpdatePlugin**插件，不需要参数。 但是，你可以指定可选参数，例如，若要指定标准 Windows 更新代理查询字符串筛选的一套通过插件应用的更新。 对于*名称*，使用**查询字符串**，以及*值*，将完全查询放在引号。<br /><br /> 有关详细信息，请参阅[如何群集的更新插件工作](cluster-aware-updating-plug-ins.md)。|  
  
##  <a name="BKMK_runtime"></a>指定你请求更新运行时的选项  
 下表列出了请求更新运行时，你可以指定的选项（而不是更新运行配置文件中的这些）。 有关可在更新运行个人资料设置选项的信息，请参阅上的表。  
  
|选项|默认值|详细信息|  
|------------|-------------------|-------------|  
|**群集名称**|无 <br>**注意：**仅当 CAU UI 无法运行上故障转移群集节点中，或你想要参考故障转移群集不同于运行 CAU UI 时，必须设置此选项。|对其执行更新运行群集 NetBIOS 名称。|  
|**凭据**|当前帐户凭据|管理凭据目标群集上的更新运行将来执行。 你可能已经必要的凭据如果启动 CAU UI（或者如果你使用的 CAU PowerShell cmdlet 打开 PowerShell 会话，）从群集具有管理员权限的帐户。|  
|**NodeOrder**|默认情况下，CAU 开头节点拥有小数聚集角色，然后前进到第二小数，依此类推具有节点。|姓名，应该会更新（如果可能）的顺序群集节点。|  
  
##  <a name="BKMK_profile"></a>使用更新运行配置文件  
 每个更新运行可能与特定更新运行个人资料。 更新运行配置文件存储在默认*%windir%\cluster*文件夹。 如果你使用的 CAU UI 远程更新过程中模式中，你可以指定你应用更新，次更新运行配置文件，也可以使用默认更新运行配置文件。 如果你使用的 CAU 自行更新模式中，你可以从导入设置指定更新运行个人资料时配置自行更新的选项。 在这两种情况下，你可以根据你的需求覆盖显示运行更新选项的值。 如果你希望，你可以将更新运行选项另存为更新运行配置文件与同名文件或不同的文件名。 下一次，你将更新应用，或配置自行更新的选项，CAU 自动选择更新运行个人资料以前选择。  
  
 你可以修改现有更新运行配置文件，或创建一个新通过选择**创建修改更新运行个人资料或**CAU 用户界面中。

下面是有关使用更新运行配置文件的一些重要说明：

* 更新运行配置文件不会存储如管理凭据群集特定信息。 如果你使用的 CAU 型自行更新，更新运行个人资料也不会存储自更新计划信息。 这样可以跨指定的类别中的所有故障转移群集共享更新运行配置文件。
* 如果配置自行更新使用更新运行个人资料的选项，稍后修改的配置文件的不同运行更新选项的值自行更新配置不会自动更改。 若要将新的更新运行设置应用，必须再次配置自行更新选项。
* 运行个人资料编辑器遗憾的是不支持包含空间，如的文件路径*C:\Program 文件*。 作为解决方法，存储你预，并在路径中不包括空间，或 PowerShell 专门用于管理运行的配置文件，掌握引号路径运行时的文章更新脚本**调用 CauRun**。

### <a name="windows-powershell-equivalent-commands"></a>Windows PowerShell 等效命令
  
 你可以从导入设置更新运行个人资料运行时**调用 CauRun**，**添加 CauClusterRole**，或**组 CauClusterRole** cmdlet。  
  
 下面的示例执行扫描和完整更新上运行名为群集*CONTOSO FC1*，使用中指定的更新运行选项*C:\Windows\Cluster\DefaultParameters.xml*。 其余 cmdlet 参数的情况下使用默认值。  
  
```powershell  
$MyRunProfile = Import-Clixml C:\Windows\Cluster\DefaultParameters.xml  
Invoke-CauRun –ClusterName CONTOSO-FC1 @MyRunProfile   
```  
  
 通过使用更新运行个人资料，您可以使用一致设置的例外管理、时间界限和运营的其他参数更新中重复故障转移群集。 由于这些设置通常特定于的故障转移群集类，如"所有 Microsoft SQL Server 群集"或"我的关键业务群集"，你可能想要命名为每个更新运行配置文件与将使用它的故障转移群集类根据。 此外，你可能想要管理更新运行上的配置文件的故障转移群集特定类 IT 组织中的所有访问的文件共享。  
  
  
  
## <a name="see-also"></a>请参阅

-   [群集的更新](cluster-aware-updating.md)
  
-   [在 Windows PowerShell 群集的更新 Cmdlet](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating)