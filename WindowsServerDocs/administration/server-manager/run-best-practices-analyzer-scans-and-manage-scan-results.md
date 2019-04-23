---
title: 运行最佳做法分析器扫描并管理扫描结果 _1
description: 服务器管理器
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 58fdf35e1d06fe7176b122155b2768094f2b01b4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838218"
---
# <a name="run-best-practices-analyzer-scans-and-manage-scan-results"></a>运行最佳做法分析器扫描并管理扫描结果

>适用于：Windows Server 2016

在 Windows 管理中， *最佳做法* 是通常情况下由专家定义的采用理想方式配置服务器的指南。 例如，对于大多数服务器应用程序来说，只打开这些应用程序与其他联网计算机通信所需的端口并阻止未使用的端口会被视为最佳做法。 尽管违反最佳做法，甚至是违反关键的最佳做法，但也不一定就会发生问题；那只是表示服务器配置可能会导致性能降低、可靠性差、意外冲突、增加了安全风险或其他潜在问题。

最佳做法分析器 (BPA) 是 Windows Server 2012 R2、 Windows Server 2012 和 Windows Server 2008 R2 中可用的服务器管理工具。 BPA 可帮助管理员减少违反最佳做法通过扫描或 Windows Server 2008 R2，在运行 Windows Server 2012 的托管的服务器安装的角色并报告给管理员的最佳做法违反情况。

您可以运行最佳做法分析器 (BPA) 扫描可以从服务器管理器中，通过使用 BPA GUI 中，或使用 Windows PowerShell 中的 cmdlet。 从 Windows Server 2012 开始，你可以扫描一个或多个角色，一次多个服务器上是否在服务器管理器控制台中使用的最佳实践分析工具磁贴或 Windows PowerShell cmdlet 运行扫描。 你还可以指示 BPA 排除或忽略不想查看的扫描结果。

本主题包含以下部分。

-   [查找 BPA](#BKMK_find)

-   [BPA 的工作原理](#BKMK_how)

-   [对角色执行最佳做法分析器扫描](#BKMK_BPAscan)

-   [管理扫描结果](#BKMK_manage)

## <a name="BKMK_find"></a>查找 BPA
有关角色的最佳实践分析工具磁贴和服务器组页面的服务器管理器中 Windows Server 2012 R2 和 Windows Server 2012 中，或者您可以使用提升的用户权限以运行最佳实践分析工具 cmdlet 打开 Windows PowerShell 会话。

## <a name="BKMK_how"></a>BPA 的工作原理
通过测量有效性、 可信赖性以及可靠性的八个不同类别中的最佳做法规则的角色符合 BPA 的工作方式。 衡量结果可以是下表所述的三种严重性级别中的任何一个。

|严重性级别|描述|
|---------|--------|
|错误|当角色不满足最佳做法规则条件时，会返回错误结果，而且可能会出现功能性问题。|
|信息|当角色满足最佳做法规则条件时，会返回信息结果。|
|警告|如果不作更改，那么不符合条件的结果会引起问题，从而返回警告的结果。 应用程序可能符合当前操作，但如果不对其配置或策略设置进行更改，就可能不满足规则条件。 例如，当角色不能使用许可证服务器时，扫描远程桌面服务可能显示警告结果，因为即使扫描时没有活动的远程连接，也不会让许可证服务器阻止新远程连接获得有效的客户端访问许可证。|

### <a name="rule-categories"></a>规则类别
下表描述了最佳做法规则类别对哪些角色期间最佳做法分析器评估扫描。

|类别名称|描述|
|---------|--------|
|安全性|安全规则用于度量受到威胁，例如未经授权或恶意用户或丢失或盗窃机密或专有数据的角色的相对风险。|
|性能|性能规则用于度量角色的功能以处理请求并在企业中预期的给定角色的工作负荷的时间段内执行其预定的职责。|
|配置|配置规则用于确定出可能需要修改以使角色实现最佳性能的角色设置。 配置规则可以帮助防止设置冲突，这些冲突可能导致错误消息或者阻止角色在企业中执行其预定职责。|
|策略|策略规则用于确定可能需要修改以最佳方式安全地运行的角色的组策略或 Windows 注册表设置。|
|操作|操作规则用于确定角色在企业中执行预定任务上可能出现的失败。|
|部署前|部署前规则是已安装角色在企业中部署前应用的规则。 这些规则可让管理员在角色进入生产使用前评估是否满足最佳做法。|
|部署后|部署后规则是在启动某个角色必需的所有服务并在企业中运行该角色后应用的规则。|
|先决条件|在 BPA 应用其他类别的特定规则之前，先决条件规则说明角色所需的配置设置、策略设置和功能。 扫描结果中的先决条件表示不正确的设置、缺少的程序、不正确地启用或禁用的策略、注册表项设置或其他配置已经阻止 BPA 在扫描期间应用一个或多个规则。 先决条件结果并不暗示是否符合最佳做法。 这意味着某个规则可能不会得到应用，因此不属于扫描结果的一部分。|

## <a name="BKMK_BPAscan"></a>对角色执行最佳做法分析器扫描
您可以执行 BPA 扫描角色上使用 BPA GUI 中服务器管理器中，或通过使用 Windows PowerShell cmdlet。

在 Windows Server 2012 R2 和 Windows Server 2012 中，部分角色会提示您指定其他参数，如特定服务器或在启动 BPA 扫描前运行的角色的部分或子模型 Id 的共享的名称。 要对需要指定附加参数的模型执行 BPA 扫描，请使用 BPA cmdlet；BPA GUI 不能接受子模型 ID 等附加参数。 例如，子模型 ID **FSRM** 表示文件服务器资源管理器（文件和存储服务的角色服务）的文件服务 BPA 子模型。 若要仅在文件服务器资源管理器角色服务上，通过使用 Windows PowerShell cmdlet 运行 BPA 扫描，并将参数添加运行一次扫描`SubmodelId`到 cmdlet。

尽管不能将附加参数传递给在 BPA GUI 中启动的扫描，但服务器管理器中的 BPA 磁贴显示最近 BPA 扫描，而不管扫描启动方式的结果。

-   [使用 BPA GUI 扫描角色](#BKMK_GUIscan)

-   [使用 Windows PowerShell cmdlet 扫描角色](#BKMK_PSscan)

### <a name="BKMK_GUIscan"></a>使用 BPA GUI 扫描角色
按照以下步骤操作可使用 BPA GUI 扫描一个或多个角色。

##### <a name="to-scan-roles-by-using-the-bpa-gui"></a>使用 BPA GUI 扫描角色的步骤

1.  执行下列操作之一以打开服务器管理器中，如果它不是已打开。

    -   在 Windows 任务栏上，单击服务器管理器按钮。

    -   上**启动**屏幕上，单击服务器管理器磁贴。

2.  在导航窗格中，打开角色或组页面。

    从角色或组页面运行 BPA 扫描可以扫描该组中服务器上安装的所有角色。

3.  上**任务**菜单**最佳做法分析器**磁贴中，单击**启动 BPA 扫描**。

4.  BPA 扫描可能需要几分钟才能完成，具体取决于评估选中的角色或组的规则数量。

### <a name="BKMK_PSscan"></a>使用 Windows PowerShell cmdlet 扫描角色
使用以下过程通过使用 Windows PowerShell cmdlet 扫描一个或多个角色。

> [!NOTE]
> 此部分中的过程不显示所有 BPA cmdlet 和参数。 有关 Windows PowerShell 中 BPA 操作的详细信息，在 Windows PowerShell 会话中，输入**获取帮助***BPACmdlet***-完整**，其中*BPACmdlet*可以是以下值。 此外可以上查找 BPA cmdlet 帮助主题[Windows Server 技术中心](https://go.microsoft.com/fwlink/p/?LinkId=240177)。

-   **Get-BPAmodel**

-   **Invoke-BPAmodel**

-   **Get-BPAResult**

-   **Set-BPAResult**

#### <a name="BKMK_singlerole"></a>若要使用 Windows PowerShell cmdlet 扫描一个角色

1.  执行下列操作之一以使用提升的用户权限运行 Windows PowerShell。

    -   若要以管理员身份从运行 Windows PowerShell**启动**屏幕上，右键单击**Windows PowerShell**中的磁贴**应用**结果，并在应用栏上，单击**以管理员身份运行**。

    -   若要从桌面以管理员身份运行 Windows PowerShell，右键单击**Windows PowerShell**快捷方式在任务栏中，然后单击**以管理员身份运行**。

2.  从 Windows PowerShell 3.0 开始，cmdlet 模块自动导入到你的 Windows PowerShell 会话首次使用模块中的 cmdlet。 无须导入或加载 BPA cmdlet 模块。

3.  查找对其执行 BPA 扫描可以是通过输入的所有角色的模型 Id **Get-bpamodel** cmdlet，如下面的示例中所示。

    `Get-Bpamodel`

4.  在步骤 3 的结果中，查找要对其运行 BPA 扫描的角色的模型 ID。

5.  输入以下命令之一以启动特定角色的 BPA 扫描。 对于多个角色的情况，用逗号分隔模型 ID。

    `Invoke-BPAmodel -modelId <modelID_from_Step3>`

    `Invoke-BPAmodel <modelID_from_Step3>`

    **示例：**`Invoke-BPAmodel -modelId Microsoft/Windows/DNSServer,Microsoft/Windows/FileServices`

    > [!NOTE]
    > 模型 ID 包括 **Id** 列中显示的整条路径，如 **Microsoft/Windows/Hyper-V**。

    你还可以从步骤 3 的结果中在特定角色上启动扫描，方法是将 `Get-BPAmodel` cmdlet 的结果通过管道传输到 `Invoke-BPAmodel` cmdlet，如以下示例所示。

    `Get-BPAmodel <model_ID> | Invoke-BPAmodel`

    运行此 cmdlet 但不指定模型 ID 的所有模型都返回都传输`Get-BPAmodel`到 cmdlet `Invoke-BPAmodel` cmdlet，已添加到服务器管理器服务器池的服务器可用的所有模型上启动扫描。

#### <a name="BKMK_allroles"></a>若要使用 Windows PowerShell cmdlet 扫描所有角色

1.  如果尚未打开，请使用提升的用户权限打开 Windows PowerShell 会话。 有关说明，请参阅以上过程。

2.  将可以对其执行 BPA 扫描的所有角色通过管道传输到 `Invoke-BPAmodel` cmdlet 中以启动扫描。

    `Get-BPAmodel | Invoke-BPAmodel`

3.  扫描完成后，则 Windows PowerShell 将返回类似于以下内容，每个角色已扫描的结果。

    ```
    modelId                 :  Microsoft/Windows/FileServices
    SubmodelId              :
    Success                 :  True
    Scantime                :  1/01/2012  12:18:40 PM
    ScantimeUtcOffset       :  -08:00:00
    detail                  :  {server_name1, server_name2}

    ```

## <a name="BKMK_manage"></a>管理扫描结果
GUI 中的 BPA 扫描完成后，你可以在 BPA 磁贴中查看扫描结果。 当你在磁贴中选择一个结果时，磁贴中的预览窗格会显示结果属性，包括角色是否符合相关的最佳做法的指示。 如果结果不符合，并且你想要了解如何解决结果属性中所述的问题，错误和警告结果属性中的超链接可打开 Windows Server TechCenter 上的详细的解决方案帮助主题。

> [!NOTE]
> BPA 扫描结果不能自动保存或存档。 对模型或子模型运行新的扫描会覆盖上一次扫描的结果。 若要保存 BPA 扫描结果以供存档、打印或发送给他人，请参阅本节中的[以不同格式从 Windows PowerShell 会话查看 BPA 结果](#BKMK_formats)。

### <a name="exclude-and-include-bpa-results"></a>排除和包括 BPA 结果
如果不需要查看部分 BPA 结果，如的 BPA 扫描中经常出现但无需解决，结果可以通过在 Windows PowerShell 中使用 BPA GUI 或 BPA cmdlet 来排除在结果中。 可以随时再次包括这些扫描结果。

> [!NOTE]
> 当你排除这些扫描结果时，它们也被从托管服务器的视图中排除。 其他管理员不会在托管服务器上看到排除的结果。 若要从本地服务器管理器控制台中的视图排除结果，请创建自定义查询，而不是使用**排除结果**命令。

#### <a name="BKMK_exclude"></a>排除扫描结果
“排除”  设置具有永久性；你排除的结果在同一计算机上的同一模型的未来扫描中仍将被排除，除非再次包括它们。

可以通过将 `Set-BPAResult` cmdlet 与 `Exclude` 参数一起使用来排除扫描结果。 如下所示在服务器管理器的最佳做法分析器磁贴，你可以排除单个结果对象，或也可以排除其字段 （类别、 标题和严重性，例如） 等于或包含指定的值的结果集。 例如，你可以从某个模型的扫描结果集中排除所有“性能”结果。

> [!NOTE]
> 你必须对一个模型至少运行一次 BPA 扫描，然后才能使用本部分中的过程。

###### <a name="to-exclude-scan-results-by-using-the-gui"></a>使用 GUI 排除扫描结果的步骤

1.  服务器管理器中打开角色或服务器组页面。

2.  在角色或服务器组的最佳做法分析器磁贴中，右键单击某个结果在列表中，然后依次**排除结果**。

    该结果不再在结果列表中显示。

3.  若要在 GUI 中查看排除的结果，请运行内置的“排除结果”查询。 单击“已保存的搜索查询” ，再单击“排除的结果” 。

    运行“排除的结果”查询后，注意图块副标题文本（列表中显示的结果的描述）更改为“排除的结果”。 仅已排除的结果会显示在列表中。

###### <a name="to-exclude-scan-results-by-using-windows-powershell-cmdlets"></a>使用 Windows PowerShell cmdlet 排除扫描结果的步骤

1.  使用提升的用户权限打开 Windows PowerShell 会话。

2.  通过运行以下命令从某个模型扫描中排除特定结果。

    `Get-BPAResult -modelId <model ID> | Where { $_.<Field Name> -eq "Value"} | Set-BPAResult -Exclude $true`

    前一个命令检索由表示的模型 id 的 BPA 扫描结果项*模型 ID*。

    此命令的第二个部分筛选 `Get-BPAResult` cmdlet 的结果，以仅检索结果字段（ *字段名称*表示）的值与引号中的文本相匹配的扫描结果。

    此命令的最后一个部分（即位于第二个管道字符后面的部分）排除该 cmdlet 的前一部分筛选的结果。

    **示例：**`Get-BPAResult -Microsoft/Windows/FileServices | Where { $_.Severity -eq "Information"} | Set-BPAResult -Exclude $true`

#### <a name="include-scan-results"></a>包括扫描结果
当你想查看已排除的扫描结果时，你可以包括这些扫描结果。 “包括”  设置具有永久性；包括的结果在同一计算机上的同一模型的未来扫描中仍保持包括状态。

##### <a name="BKMK_gui"></a>通过使用 GUI 包括扫描结果

1.  服务器管理器中打开角色或服务器组页面。

2.  在角色或服务器组的最佳做法分析器磁贴中，右键单击中排除的结果**排除的结果**查询列表，然后依次**包括结果**。

    该结果不再在已排除的结果列表中显示。 通过单击“全部清除”  来清除查询，以查看所有包括的结果列表中包括的结果。

##### <a name="BKMK_cmdlets"></a>使用 Windows PowerShell cmdlet 包括扫描结果

1.  使用提升的用户权限打开 Windows PowerShell 会话。

2.  通过键入以下命令，然后按 **Enter**来包括模型扫描中的特定结果。

    `Get-BPAResult -modelId <model Id> | Where { $_.<Field Name> -eq "Value" } | Set-BPAResult -Exclude $false`

    前一个命令检索表示的模型的 BPA 扫描结果项*模型 Id*。

    该命令中，第一个管道字符后面的第二部分 ( **|** ) 筛选的结果**Get-bparesult** cmdlet 以仅这些扫描结果为其检索结果的值所表示的字段*字段名*，与引号中的文本相匹配。

    此命令的最后一个部分（即位于第二个管道字符后面的部分）包括该 cmdlet 的第二部分筛选的结果，方法是将 **-Exclude** 参数的值设置为 **false**。

    **示例：**`Get-BPAResult -Microsoft/Windows/FileServices | Where { $_.Severity -eq "Information"} | Set-BPAResult -Exclude $false`

### <a name="view-and-export-bpa-scan-results-in-windows-powershell"></a>在 Windows PowerShell 中查看和导出 BPA 扫描结果
若要查看和使用 Windows PowerShell cmdlet 管理扫描结果，请参阅下面的过程。 在你可以使用以下任一过程之前，请对至少一个模型或子模型运行至少一次 BPA 扫描。

#### <a name="BKMK_recentPS"></a>若要使用 Windows PowerShell 查看角色的最新的扫描结果

1.  使用提升的用户权限打开 Windows PowerShell 会话。

2.  获得指定的模型 ID 的最新扫描结果。 键入的以下命令，在其中模型由表示*模型 ID*，然后按**Enter**。 可以获得多个模型 ID 的结果，并用逗号来分隔模型 ID。

    `Get-BPAResult <model ID>`

    **示例：** `Get-BPAResult Microsoft/Windows/DNSServer,Microsoft/Windows/FileServices`

    如果扫描子模型的模型，如角色服务，获得的结果对于该子仅通过在 cmdlet 中包括子模型 ID。

    **示例：** `Get-BPAResult Microsoft/Windows/FileServices -SubmodelID FSRM`

#### <a name="BKMK_formats"></a>若要查看或从 Windows PowerShell 会话中不同的格式保存 BPA 结果

-   在 Windows PowerShell 中，每个 BPA 结果如下所示。

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

        **示例：**`Get-BPAResult Microsoft/Windows/FileServices | format-Table -Property modelId,SubmodelId,computerName,Source,Severity,Category,Title,Problem,Impact,Resolution,compliance,help`

    -   若要将 BPA 结果设置为基于 GUI 网格查看器格式，具有文本字符串筛选器和列标题（单击列标题可以对结果进行排序），请运行以下 cmdlet。

        `Get-BPAResult <model ID> | OGV`

    -   若要将 BPA 结果导出到的 HTML 文件，可存档或发送电子邮件收件人，请运行以下 cmdlet，其中*路径*表示想要将 HTML 结果保存到的路径和文件名称。

        `Get-BPAResult <model ID> | convertTo-Html | Set-Content <path>`

        **示例：**`Get-BPAResult Microsoft/Windows/FileServices | convertTo-Html | Set-Content C:\BPAResults\FileServices.htm`

    -   若要将 BPA 结果导出到逗号分隔值 (CSV) 文本文件，运行以下 cmdlet，其中*路径*表示你想要将 CSV 结果保存路径和的文件名。 CSV 结果可以导入 Microsoft Excel 或其他程序，在电子表格或网格中显示数据。

        `Get-BPAResult <model ID> | Export-CSV <path>`

        **示例：**`Get-BPAResult Microsoft/Windows/FileServices | Export-CSV C:\BPAResults\FileServices.txt`

## <a name="see-also"></a>请参阅
[最佳做法分析器解决方案内容在 Windows Server TechCenter](https://go.microsoft.com/fwlink/p/?LinkId=241597)
[筛选器、 排序和查询服务器管理器磁贴中的数据](filter-sort-and-query-data-in-server-manager-tiles.md)
[Manage Multiple，remote Servers使用服务器管理器](manage-multiple-remote-servers-with-server-manager.md)
