---
title: 运行最佳做法分析器扫描和管理扫描 Results_1
description: 服务器管理器
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 232f1c80-88ef-4a39-8014-14be788c2766
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6edd561749ea0d224058b482992d357357c12505
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383079"
---
# <a name="run-best-practices-analyzer-scans-and-manage-scan-results"></a>运行最佳做法分析器扫描并管理扫描结果

>适用于：Windows Server 2016

在 Windows 管理中， *最佳做法* 是通常情况下由专家定义的采用理想方式配置服务器的指南。 例如，对于大多数服务器应用程序来说，只打开这些应用程序与其他联网计算机通信所需的端口并阻止未使用的端口会被视为最佳做法。 尽管违反最佳做法，甚至是违反关键的最佳做法，但也不一定就会发生问题；那只是表示服务器配置可能会导致性能降低、可靠性差、意外冲突、增加了安全风险或其他潜在问题。

最佳做法分析器（BPA）是 Windows Server 2012 R2、Windows Server 2012 和 Windows Server 2008 R2 中提供的一种服务器管理工具。 BPA 可以通过扫描运行 Windows Server 2012 或 Windows Server 2008 R2 的托管服务器上安装的角色，以及向管理员报告最佳做法违规，来帮助管理员减少违反最佳做法的情况。

你可以通过使用 BPA GUI 或使用 Windows PowerShell 中的 cmdlet 来运行最佳做法分析器（BPA）服务器管理器扫描。 从 Windows Server 2012 开始，无论是使用服务器管理器控制台还是 Windows PowerShell cmdlet 中的最佳做法分析器磁贴来运行扫描，你都可以在多台服务器上一次扫描一个或多个角色。 你还可以指示 BPA 排除或忽略不想查看的扫描结果。

本主题包含以下部分。

-   [查找 BPA](#BKMK_find)

-   [BPA 的工作方式](#BKMK_how)

-   [对角色执行最佳做法分析器扫描](#BKMK_BPAscan)

-   [管理扫描结果](#BKMK_manage)

## <a name="BKMK_find"></a>查找 BPA
你可以在 Windows Server 2012 R2 和 Windows Server 2012 的服务器管理器的角色和服务器组页面上查找最佳做法分析器磁贴，也可以使用提升的用户权限打开 Windows PowerShell 会话以运行最佳做法分析器 cmdlet。

## <a name="BKMK_how"></a>BPA 的工作方式
BPA 的工作方式是在8个不同类别的有效性、可信赖性和可靠性方面，使用最佳做法规则衡量角色的符合性。 衡量结果可以是下表所述的三种严重性级别中的任何一个。

|严重性级别|描述|
|---------|--------|
|Error|当角色不满足最佳做法规则条件时，会返回错误结果，而且可能会出现功能性问题。|
|Information|当角色满足最佳做法规则条件时，会返回信息结果。|
|警告|如果不作更改，那么不符合条件的结果会引起问题，从而返回警告的结果。 应用程序可能符合当前操作，但如果不对其配置或策略设置进行更改，就可能不满足规则条件。 例如，当角色不能使用许可证服务器时，扫描远程桌面服务可能显示警告结果，因为即使扫描时没有活动的远程连接，也不会让许可证服务器阻止新远程连接获得有效的客户端访问许可证。|

### <a name="rule-categories"></a>规则类别
下表描述了在最佳做法分析器扫描期间对哪些角色进行度量的最佳做法规则类别。

|类别名称|描述|
|---------|--------|
|安全性|安全规则用于度量角色面临的威胁的相对风险，如未授权的用户或恶意用户，或者机密或专有数据丢失或被盗。|
|性能|性能规则用于度量角色在给定角色工作负荷的预期时间段内处理请求并在企业中执行其预定职责的能力。|
|配置|配置规则用于确定出可能需要修改以使角色实现最佳性能的角色设置。 配置规则可以帮助防止设置冲突，这些冲突可能导致错误消息或者阻止角色在企业中执行其预定职责。|
|策略|策略规则用于标识可能需要修改角色才能以最佳方式安全地运行的组策略或 Windows 注册表设置。|
|操作|操作规则用于确定角色在企业中执行预定任务上可能出现的失败。|
|部署前|部署前规则是已安装角色在企业中部署前应用的规则。 这些规则可让管理员在角色进入生产使用前评估是否满足最佳做法。|
|部署后|部署后规则是在启动某个角色必需的所有服务并在企业中运行该角色后应用的规则。|
|先决条件|在 BPA 应用其他类别的特定规则之前，先决条件规则说明角色所需的配置设置、策略设置和功能。 扫描结果中的先决条件表示不正确的设置、缺少的程序、不正确地启用或禁用的策略、注册表项设置或其他配置已经阻止 BPA 在扫描期间应用一个或多个规则。 先决条件结果并不暗示是否符合最佳做法。 这意味着某个规则可能不会得到应用，因此不属于扫描结果的一部分。|

## <a name="BKMK_BPAscan"></a>对角色执行最佳做法分析器扫描
你可以通过使用服务器管理器中的 BPA GUI 或使用 Windows PowerShell cmdlet 对角色执行 BPA 扫描。

在 Windows Server 2012 R2 和 Windows Server 2012 中，某些角色会提示你在启动 BPA 扫描前指定附加参数，如运行部分角色或子模型 Id 的特定服务器或共享的名称。 要对需要指定附加参数的模型执行 BPA 扫描，请使用 BPA cmdlet；BPA GUI 不能接受子模型 ID 等附加参数。 例如，子模型 ID **FSRM** 表示文件服务器资源管理器（文件和存储服务的角色服务）的文件服务 BPA 子模型。 若要仅在文件服务器资源管理器角色服务上运行扫描，请使用 Windows PowerShell cmdlet 运行 BPA 扫描，并将参数 `SubmodelId` 添加到 cmdlet。

尽管不能将其他参数传递给在 BPA GUI 中启动的扫描，但服务器管理器中的 BPA 磁贴会显示最新 BPA 扫描的结果，而与扫描的启动方式无关。

-   [使用 BPA GUI 扫描角色](#BKMK_GUIscan)

-   [使用 Windows PowerShell cmdlet 扫描角色](#BKMK_PSscan)

### <a name="BKMK_GUIscan"></a>使用 BPA GUI 扫描角色
按照以下步骤操作可使用 BPA GUI 扫描一个或多个角色。

##### <a name="to-scan-roles-by-using-the-bpa-gui"></a>使用 BPA GUI 扫描角色的步骤

1.  执行下列操作之一，打开服务器管理器（如果尚未打开）。

    -   在 Windows 任务栏上，单击 "服务器管理器" 按钮。

    -   在 "**开始**" 屏幕上，单击 "服务器管理器" 磁贴。

2.  在导航窗格中，打开角色或组页面。

    从角色或组页面运行 BPA 扫描可以扫描该组中服务器上安装的所有角色。

3.  在 "**最佳做法分析器**" 磁贴的 "**任务**" 菜单上，单击 "**启动 BPA 扫描**"。

4.  BPA 扫描可能需要几分钟才能完成，具体取决于评估选中的角色或组的规则数量。

### <a name="BKMK_PSscan"></a>使用 Windows PowerShell cmdlet 扫描角色
使用以下过程通过使用 Windows PowerShell cmdlet 扫描一个或多个角色。

> [!NOTE]
> 此部分中的过程不显示所有 BPA cmdlet 和参数。 有关 Windows PowerShell 中的 BPA 操作的详细信息，请在 Windows PowerShell 会话中输入**Get-help***BPACmdlet***-full**，其中， *BPACmdlet*可以是下列值之一。 你还可以在[Windows Server 技术中心](https://go.microsoft.com/fwlink/p/?LinkId=240177)查找 BPA cmdlet 帮助主题。

-   **Get-bpamodel**

-   **Get-bpamodel**

-   **Get-bparesult**

-   **Get-bparesult**

#### <a name="BKMK_singlerole"></a>使用 Windows PowerShell cmdlet 扫描单个角色

1.  执行下列操作之一，以提升的用户权限运行 Windows PowerShell。

    -   若要从 "**开始**" 屏幕以管理员身份运行 windows powershell，请在 "**应用**" 结果中右键单击 " **windows powershell** " 磁贴，然后单击应用栏上的 "以**管理员身份运行**"。

    -   若要从桌面以管理员身份运行 Windows PowerShell，请在任务栏中右键单击**Windows powershell**快捷方式，然后单击 "以**管理员身份运行**"。

2.  从 Windows PowerShell 3.0 开始，首次使用模块中的 cmdlet 时，cmdlet 模块将自动导入到 Windows PowerShell 会话中。 无须导入或加载 BPA cmdlet 模块。

3.  通过输入**get-bpamodel** cmdlet 查找可执行 BPA 扫描的所有角色的模型 id，如以下示例中所示。

    `Get-Bpamodel`

4.  在步骤 3 的结果中，查找要对其运行 BPA 扫描的角色的模型 ID。

5.  输入以下命令之一以启动特定角色的 BPA 扫描。 对于多个角色的情况，用逗号分隔模型 ID。

    `Invoke-BPAmodel -modelId <modelID_from_Step3>`

    `Invoke-BPAmodel <modelID_from_Step3>`

    示例：`Invoke-BPAmodel -modelId Microsoft/Windows/DNSServer,Microsoft/Windows/FileServices`

    > [!NOTE]
    > 模型 ID 包括 **Id** 列中显示的整条路径，如 **Microsoft/Windows/Hyper-V**。

    你还可以从步骤 3 的结果中在特定角色上启动扫描，方法是将 `Get-BPAmodel` cmdlet 的结果通过管道传输到 `Invoke-BPAmodel` cmdlet，如以下示例所示。

    `Get-BPAmodel <model_ID> | Invoke-BPAmodel`

    运行此 cmdlet 但不指定模型 ID 会将 `Get-BPAmodel` cmdlet 返回的所有模型都传输到 @no__t cmdlet，开始扫描已添加到服务器管理器服务器池的服务器上可用的所有模型。

#### <a name="BKMK_allroles"></a>使用 Windows PowerShell cmdlet 扫描所有角色

1.  使用提升的用户权限打开 Windows PowerShell 会话（如果尚未打开一个）。 有关说明，请参阅以上过程。

2.  将可以对其执行 BPA 扫描的所有角色通过管道传输到 `Invoke-BPAmodel` cmdlet 中以启动扫描。

    `Get-BPAmodel | Invoke-BPAmodel`

3.  扫描完成后，Windows PowerShell 将为扫描的每个角色返回如下所示的结果。

    ```
    modelId                 :  Microsoft/Windows/FileServices
    SubmodelId              :
    Success                 :  True
    Scantime                :  1/01/2012  12:18:40 PM
    ScantimeUtcOffset       :  -08:00:00
    detail                  :  {server_name1, server_name2}

    ```

## <a name="BKMK_manage"></a>管理扫描结果
GUI 中的 BPA 扫描完成后，你可以在 BPA 磁贴中查看扫描结果。 当你在磁贴中选择一个结果时，磁贴中的预览窗格会显示结果属性，包括角色是否符合相关的最佳做法的指示。 如果结果不符合，并且你想知道如何解决结果属性中所述的问题、错误和警告结果属性中的超链接，请打开 Windows Server 技术中心上的详细解决方案帮助主题。

> [!NOTE]
> BPA 扫描结果不能自动保存或存档。 对模型或子模型运行新的扫描会覆盖上一次扫描的结果。 若要保存 BPA 扫描结果以供存档、打印或发送给他人，请参阅本节中的[以不同格式从 Windows PowerShell 会话查看 BPA 结果](#BKMK_formats)。

### <a name="exclude-and-include-bpa-results"></a>排除和包括 BPA 结果
如果不需要查看某些 BPA 结果，如 BPA 扫描中经常出现但无需解决的结果，可以使用 Windows PowerShell 中的 BPA GUI 或 BPA cmdlet 排除结果。 可以随时再次包括这些扫描结果。

> [!NOTE]
> 当你排除这些扫描结果时，它们也被从托管服务器的视图中排除。 其他管理员不会在托管服务器上看到排除的结果。 若要仅从本地服务器管理器控制台中的视图排除结果，请创建一个自定义查询，而不是使用 "**排除结果**" 命令。

#### <a name="BKMK_exclude"></a>排除扫描结果
“排除” 设置具有永久性；你排除的结果在同一计算机上的同一模型的未来扫描中仍将被排除，除非再次包括它们。

可以通过将 `Set-BPAResult` cmdlet 与 `Exclude` 参数一起使用来排除扫描结果。 与服务器管理器中的最佳做法分析器磁贴一样，你可以排除单个结果对象，也可以排除其字段（例如类别、标题和严重性）等于或包含指定值的结果集。 例如，你可以从某个模型的扫描结果集中排除所有“性能”结果。

> [!NOTE]
> 你必须对一个模型至少运行一次 BPA 扫描，然后才能使用本部分中的过程。

###### <a name="to-exclude-scan-results-by-using-the-gui"></a>使用 GUI 排除扫描结果的步骤

1.  在服务器管理器中打开角色或服务器组页面。

2.  在角色或服务器组的 "最佳做法分析器" 磁贴中，右键单击列表中的结果，然后单击 "**排除结果**"。

    该结果不再在结果列表中显示。

3.  若要在 GUI 中查看排除的结果，请运行内置的“排除结果”查询。 单击“已保存的搜索查询”，再单击“排除的结果”。

    运行“排除的结果”查询后，注意图块副标题文本（列表中显示的结果的描述）更改为“排除的结果”。 仅已排除的结果会显示在列表中。

###### <a name="to-exclude-scan-results-by-using-windows-powershell-cmdlets"></a>使用 Windows PowerShell cmdlet 排除扫描结果的步骤

1.  使用提升的用户权限打开 Windows PowerShell 会话。

2.  通过运行以下命令从某个模型扫描中排除特定结果。

    `Get-BPAResult -modelId <model ID> | Where { $_.<Field Name> -eq "Value"} | Set-BPAResult -Exclude $true`

    前面的命令检索*模型*id 表示的模型 ID 的 BPA 扫描结果项。

    此命令的第二个部分筛选 `Get-BPAResult` cmdlet 的结果，以仅检索结果字段（ *字段名称*表示）的值与引号中的文本相匹配的扫描结果。

    此命令的最后一个部分（即位于第二个管道字符后面的部分）排除该 cmdlet 的前一部分筛选的结果。

    示例：`Get-BPAResult -Microsoft/Windows/FileServices | Where { $_.Severity -eq "Information"} | Set-BPAResult -Exclude $true`

#### <a name="include-scan-results"></a>包括扫描结果
当你想查看已排除的扫描结果时，你可以包括这些扫描结果。 “包括” 设置具有永久性；包括的结果在同一计算机上的同一模型的未来扫描中仍保持包括状态。

##### <a name="BKMK_gui"></a>使用 GUI 包括扫描结果

1.  在服务器管理器中打开角色或服务器组页面。

2.  在角色或服务器组的 "最佳做法分析器" 磁贴中，右键单击 "**排除的结果**" 查询列表中的某个排除的结果，然后单击 "**包括结果**"。

    该结果不再在已排除的结果列表中显示。 通过单击“全部清除” 来清除查询，以查看所有包括的结果列表中包括的结果。

##### <a name="BKMK_cmdlets"></a>使用 Windows PowerShell cmdlet 包括扫描结果

1.  使用提升的用户权限打开 Windows PowerShell 会话。

2.  通过键入以下命令，然后按 **Enter**来包括模型扫描中的特定结果。

    `Get-BPAResult -modelId <model Id> | Where { $_.<Field Name> -eq "Value" } | Set-BPAResult -Exclude $false`

    前面的命令检索*模型 Id*表示的模型的 BPA 扫描结果项。

    命令的第二部分在第一个管道字符（ **|** ）之后筛选**get-bparesult** cmdlet 的结果，以仅检索结果字段（由*字段名称*表示）的值匹配的扫描结果用引号引起来的文本。

    此命令的最后一个部分（即位于第二个管道字符后面的部分）包括该 cmdlet 的第二部分筛选的结果，方法是将 **-Exclude** 参数的值设置为 **false**。

    示例：`Get-BPAResult -Microsoft/Windows/FileServices | Where { $_.Severity -eq "Information"} | Set-BPAResult -Exclude $false`

### <a name="view-and-export-bpa-scan-results-in-windows-powershell"></a>在 Windows PowerShell 中查看和导出 BPA 扫描结果
若要使用 Windows PowerShell cmdlet 查看和管理扫描结果，请参阅以下过程。 在你可以使用以下任一过程之前，请对至少一个模型或子模型运行至少一次 BPA 扫描。

#### <a name="BKMK_recentPS"></a>使用 Windows PowerShell 查看最新的角色扫描结果

1.  使用提升的用户权限打开 Windows PowerShell 会话。

2.  获得指定的模型 ID 的最新扫描结果。 键入以下项，其中模型由*模型 ID*表示，再按**enter**。 可以获得多个模型 ID 的结果，并用逗号来分隔模型 ID。

    `Get-BPAResult <model ID>`

    示例：`Get-BPAResult Microsoft/Windows/DNSServer,Microsoft/Windows/FileServices`

    如果扫描了模型的子模型（如角色服务），则通过将子模型 ID 包括在 cmdlet 中，只获取该子模型的结果。

    示例：`Get-BPAResult Microsoft/Windows/FileServices -SubmodelID FSRM`

#### <a name="BKMK_formats"></a>若要从 Windows PowerShell 会话中查看或保存不同格式的 BPA 结果

-   在 Windows PowerShell 中，每个 BPA 结果都如下所示。

    ```
    ResultNumber     :  14
    ResultId         :  1557706192
    modelId          :  Microsoft/Windows/FileServices
    SubmodelId       :  FSRM
    RuleId           :  16
    computerName     :  server_name1, server_name2
    Context          :  FileServices
    Source           :  server_name1
    Severity         :  Information
    Category         :  Configuration
    Title            :  Access Denied remediation requires remote management be enabled on this server
    Problem          :
    Impact           :
    Resolution       :
    compliance       :  The File Server Best Practices Analyzer scan has determined that you are in compliance with this best practice.
    help             :
    Excluded         :  False

    ```

    请执行以下操作之一。

    -   若要将 BPA 结果设置为表格格式，请运行以下 cmdlet，添加你想在以上示例中查看的结果属性。

        `Get-BPAResult model ID | format-Table -Property <property1,property2,property3...>`

        示例：`Get-BPAResult Microsoft/Windows/FileServices | format-Table -Property modelId,SubmodelId,computerName,Source,Severity,Category,Title,Problem,Impact,Resolution,compliance,help`

    -   若要将 BPA 结果设置为基于 GUI 网格查看器格式，具有文本字符串筛选器和列标题（单击列标题可以对结果进行排序），请运行以下 cmdlet。

        `Get-BPAResult <model ID> | OGV`

    -   若要将 BPA 结果导出到可存档或发送给电子邮件收件人的 HTML 文件，请运行以下 cmdlet，其中*path*表示要将 HTML 结果保存到的路径和文件名。

        `Get-BPAResult <model ID> | convertTo-Html | Set-Content <path>`

        示例：`Get-BPAResult Microsoft/Windows/FileServices | convertTo-Html | Set-Content C:\BPAResults\FileServices.htm`

    -   若要将 BPA 结果导出到逗号分隔值（CSV）文本文件，请运行以下 cmdlet，其中*path*表示要将 CSV 结果保存到的路径和文件名。 CSV 结果可以导入到 Microsoft Excel 或其他以电子表格或网格显示数据的程序。

        `Get-BPAResult <model ID> | Export-CSV <path>`

        示例：`Get-BPAResult Microsoft/Windows/FileServices | Export-CSV C:\BPAResults\FileServices.txt`

## <a name="see-also"></a>请参阅
[Windows Server 技术中心上的最佳做法分析器解析内容](https://go.microsoft.com/fwlink/p/?LinkId=241597)@no__t[服务器管理器磁贴中筛选、排序和查询数据](filter-sort-and-query-data-in-server-manager-tiles.md)
[管理多台远程服务器，服务器管理器](manage-multiple-remote-servers-with-server-manager.md)
