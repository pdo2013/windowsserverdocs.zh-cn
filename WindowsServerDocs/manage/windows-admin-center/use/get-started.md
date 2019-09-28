---
title: Windows 管理中心入门
description: Windows 管理中心入门
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 02/15/2019
ms.openlocfilehash: 68b5c7b2c5bc8e93d653514b2664d96b97b07a9e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406849"
---
# <a name="get-started-with-windows-admin-center"></a>Windows 管理中心入门

>适用于：Windows Admin Center、Windows Admin Center 预览版

> [!Tip]
> 不熟悉 Windows Admin Center？
> [了解有关 Windows Admin Center 的更多信息](../understand/windows-admin-center.md)或[立即下载](https://aka.ms/windowsadmincenter)。

## <a name="windows-admin-center-installed-on-windows-10"></a>Windows 10 上安装的 windows 管理中心

> [!IMPORTANT]
> 你必须是本地管理员组的成员才能在 Windows 10 上使用 Windows 管理中心

### <a name="selecting-a-client-certificate"></a>选择客户端证书

首次在 Windows 10 上打开 Windows 管理中心时, 请确保选择*Windows 管理中心客户端*证书 (否则, 会收到 HTTP 403 错误, 指出 "无法访问此页")。

在 Microsoft Edge 中, 当系统提示此对话框时:
 
1. 单击 "**更多选项**"

    ![](../media/launch-cert-1.png)

2. 选择标记为**Windows 管理中心客户端**的证书, 然后单击 **"确定"**

    ![](../media/launch-cert-2.png)

3. 请确保选择 "**始终允许访问**", 然后单击 "**允许**"

    ![](../media/launch-cert-3.png)

## <a name="connecting-to-managed-nodes-and-clusters"></a>连接到托管节点和群集

在您完成 Windows 管理中心的安装后, 您可以从主概述页添加服务器或群集进行管理。

 **添加单个服务器或群集作为托管节点**

1. 单击 "**所有连接**" 下的 " **+ 添加**"。

   ![](../media/launch/addserver0.png)

2. 选择添加服务器、故障转移群集或超聚合群集连接:
    
   ![](../media/launch/addserver1.png)

3. 键入要管理的服务器或群集的名称, 然后单击 "**提交**"。 服务器或群集将添加到 "概述" 页上的 "连接" 列表中。

   ![](../media/launch/addserver2.png)

   **--或--**

**大容量导入多台服务器**

 1. 在 "**添加服务器连接**" 页上, 选择 "**导入服务器**" 选项卡。

    ![](../media/launch/import-servers.png)

 2. 单击 "**浏览**" 并选择一个文本文件, 其中包含要添加的服务器的逗号或换行符的列表。

> [!Note]
> 通过[使用 PowerShell 导出连接](#use-powershell-to-import-or-export-your-connections-with-tags)创建的 .csv 文件包含除服务器名称之外的其他信息, 与此导入方法不兼容。

  **--或--**

**通过搜索来添加服务器 Active Directory**

 1. 在 "**添加服务器连接**" 页上, 选择 "**搜索 Active Directory** " 选项卡。

    ![](../media/launch/search-ad.png)

 2. 输入搜索条件, 然后单击 "**搜索**"。 支持通配符 (*)。

 3. 搜索完成后-选择一个或多个结果, 可以选择添加标记, 然后单击 "**添加**"。

## <a name="authenticate-with-the-managed-node"></a>通过托管节点进行身份验证 ##

Windows 管理中心支持通过多种机制对托管节点进行身份验证。 默认值为单一登录。

**单一登录**

您可以使用您当前的 Windows 凭据对托管节点进行身份验证。 这是默认设置, Windows 管理中心会在你添加服务器时尝试登录。 

**在 Windows Server 上部署为服务时的单一登录**

如果在 Windows Server 上安装了 Windows 管理中心, 则需要进行其他配置才能进行单一登录。  [为委派配置环境](../configure/user-access-control.md)

**--或--**

**使用 "*管理身份*" 指定凭据**

在 "**所有连接**" 下, 从列表中选择服务器, 然后选择 "**管理**身份" 以指定将用于向托管节点进行身份验证的凭据:

![](../media/launch-use-6.png)

如果 Windows 管理中心正在 Windows Server 上的服务模式下运行, 但未配置 Kerberos 委托, 则必须重新输入您的 Windows 凭据:

![](../media/launch-use-7.png)

你可以将凭据应用于所有连接, 这将为该特定浏览器会话缓存这些凭据。 如果重新加载浏览器, 则必须重新输入作为凭据的**管理**。

**本地管理员密码解决方案 (LAPS)**

如果你的环境使用[LAPS](https://technet.microsoft.com/mt227395.aspx), 并且在 WINDOWS 10 电脑上安装了 windows 管理中心, 则可以使用 LAPS 凭据通过托管节点进行身份验证。 **如果使用此方案, 请**[提供反馈](http://aka.ms/WACFeedback)。

## <a name="using-tags-to-organize-your-connections"></a>使用标记来组织连接

您可以使用标记在连接列表中标识和筛选相关服务器。  这样, 你就可以在 "连接" 列表中查看服务器的子集。  如果有多个连接, 此方法特别有用。

### <a name="edit-tags"></a>编辑标记

* 在 "所有连接" 列表中选择一个或多个服务器
* 在 "**所有连接**" 下, 单击 "**编辑标记**"

![](../media/launch/tags-5.png)

"**编辑连接标记**" 窗格允许您从所选连接中修改、添加或删除标记:

* 若要向所选连接添加新标记, 请选择 "**添加标记**", 然后输入要使用的标记名称。

* 若要使用现有标记名称标记所选连接, 请选中要应用的标记名称旁的复选框。

* 若要从所有选择的连接中删除标记, 请取消选中要删除的标记旁边的复选框。

* 如果将标记应用于所选连接的子集, 则复选框将显示为中间状态。 您可以单击复选框将其选中, 并将标记应用于所有选定的连接, 或再次单击以取消选中它并从所有选定连接中删除标记。

![](../media/launch/tags-6.png)

### <a name="filter-connections-by-tag"></a>按标记筛选连接

向一个或多个服务器连接添加标记后, 可以查看连接列表中的标记, 并按标记筛选连接列表。

* 若要按标记进行筛选, 请选择 "搜索" 框旁边的 "筛选器" 图标。
![](../media/launch/tags-7.png)
* 您可以选择 "or"、"and" 或 "not" 来修改所选标记的筛选器行为。
![](../media/launch/tags-8.png)

## <a name="use-powershell-to-import-or-export-your-connections-with-tags"></a>使用 PowerShell 导入或导出连接 (带有标记)

```powershell
# Load the module
Import-Module "$env:ProgramFiles\windows admin center\PowerShell\Modules\ConnectionTools"
# Available cmdlets: Export-Connection, Import-Connection

# Export connections (including tags) to .csv files
Export-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv"
# Import connections (including tags) from .csv files
Import-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv"
```

### <a name="csv-file-format-for-importing-connections"></a>用于导入连接的 CSV 文件格式

CSV 文件的格式以四个标题```"name","type","tags","groupId"```开头, 后跟一个新行的每个连接。

**name**是连接的 FQDN

**类型**为连接类型。 对于 Windows 管理中心附带的默认连接, 你将使用以下项之一:

| 连接类型 | 连接字符串 |
|------|-------------------------------|
| Windows Server | msft. 类型. 服务器 |
| Windows 10 电脑 | msft. 类型. windows-客户端 |
| 故障转移群集 | msft. 连接类型. 群集 |
| 超聚合群集 | msft. 类型. 超聚合群集 |

**标记**是用竖线分隔的。

**groupId**用于共享连接。 使用此列```global```中的值可将其设为共享连接。

### <a name="example-csv-file-for-importing-connections"></a>导入连接的示例 CSV 文件

```
"name","type","tags","groupId"
"myServer.contoso.com","msft.sme.connection-type.server","hyperv"
"myDesktop.contoso.com","msft.sme.connection-type.windows-client","hyperv"
"teamcluster.contoso.com","msft.sme.connection-type.cluster","legacyCluster|WS2016","global"
"myHCIcluster.contoso.com,"msft.sme.connection-type.hyper-converged-cluster","myHCIcluster|hyperv|JIT|WS2019"
"teamclusterNode.contoso.com","msft.sme.connection-type.server","legacyCluster|WS2016","global"
"myHCIclusterNode.contoso.com","msft.sme.connection-type.server","myHCIcluster|hyperv|JIT|WS2019"
```

## <a name="import-rdcman-connections"></a>导入 RDCman 连接

使用以下脚本将[RDCman](https://blogs.technet.microsoft.com/rmilne/2014/11/19/remote-desktop-connection-manager-download-rdcman-2-7/)中保存的连接导出到文件。 然后, 你可以将该文件导入到 Windows 管理中心, 并使用标记来维护你的 RDCMan 分组层次结构。 试试吧!

1. 将以下代码复制并粘贴到 PowerShell 会话中:

   ```powershell
   #Helper function for RdgToWacCsv
   function AddServers {
    param (
    [Parameter(Mandatory = $true)]
    [Xml.XmlLinkedNode]
    $node,
    [Parameter()]
    [String[]]
    $tags,
    [Parameter(Mandatory = $true)]
    [String]
    $csvPath
    )
    if ($node.LocalName -eq 'server') {
        $serverName = $node.properties.name
        $tagString = $tags -join "|"
        Add-Content -Path $csvPath -Value ('"'+ $serverName + '","msft.sme.connection-type.server","'+ $tagString +'"')
    } 
    elseif ($node.LocalName -eq 'group' -or $node.LocalName -eq 'file') {
        $groupName = $node.properties.name
        $tags+=$groupName
        $currNode = $node.properties.NextSibling
        while ($currNode) {
            AddServers -node $currNode -tags $tags -csvPath $csvPath
            $currNode = $currNode.NextSibling
        }
    } 
    else {
        # Node type isn't relevant to tagging or adding connections in WAC
    }
    return
   }

   <#
   .SYNOPSIS
   Convert an .rdg file from Remote Desktop Connection Manager into a .csv that can be imported into Windows Admin Center, maintaining groups via server tags. This will not modify the existing .rdg file and will create a new .csv file

    .DESCRIPTION
    This converts an .rdg file into a .csv that can be imported into Windows Admin Center.

    .PARAMETER RDGfilepath
    The path of the .rdg file to be converted. This file will not be modified, only read.

    .PARAMETER CSVdirectory
    Optional. The directory you wish to export the new .csv file. If not provided, the new file is created in the same directory as the .rdg file.

    .EXAMPLE
    C:\PS> RdgToWacCsv -RDGfilepath "rdcmangroup.rdg"
    #>
   function RdgToWacCsv {
    param(
        [Parameter(Mandatory = $true)]
        [String]
        $RDGfilepath,
        [Parameter(Mandatory = $false)]
        [String]
        $CSVdirectory
    )
    [xml]$RDGfile = Get-Content -Path $RDGfilepath
    $node = $RDGfile.RDCMan.file
    if (!$CSVdirectory){
        $csvPath = [System.IO.Path]::GetDirectoryName($RDGfilepath) + [System.IO.Path]::GetFileNameWithoutExtension($RDGfilepath) + "_WAC.csv"
    } else {
        $csvPath = $CSVdirectory + [System.IO.Path]::GetFileNameWithoutExtension($RDGfilepath) + "_WAC.csv"
    }
    New-item -Path $csvPath
    Add-Content -Path $csvPath -Value '"name","type","tags"'
    AddServers -node $node -csvPath $csvPath
    Write-Host "Converted $RDGfilepath `nOutput: $csvPath"
   }
   ```

2. 来创建。CSV 文件, 请运行以下命令:

   ```powershell
   RdgToWacCsv -RDGfilepath "path\to\myRDCManfile.rdg"
   ```

3. 导入生成的。CSV 文件中的 CSV 文件, 所有 RDCMan 分组层次结构都将由连接列表中的标记表示。 有关详细信息, 请参阅[使用 PowerShell 导入或导出连接 (带有标记)](#use-powershell-to-import-or-export-your-connections-with-tags)。

## <a name="view-powershell-scripts-used-in-windows-admin-center"></a>查看 Windows 管理中心中使用的 PowerShell 脚本

连接到服务器、群集或 PC 后, 可以查看支持 Windows 管理中心中可用 UI 操作的 PowerShell 脚本。 从工具中, 单击顶部应用程序栏中的 PowerShell 图标。 从下拉列表中选择一种感兴趣的命令, 导航到相应的 PowerShell 脚本。

![](../media/launch/showscript.png)