---
title: Get started with Windows Admin Center 入门
description: Get started with Windows Admin Center 入门
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 02/15/2019
ms.openlocfilehash: f4fd9f69e75ed80bbdb345b4041c2337c65ec2e6
ms.sourcegitcommit: 2977c707a299929c6ab0d1e0adab2e1c644b8306
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2019
ms.locfileid: "63742671"
---
# <a name="get-started-with-windows-admin-center"></a>Get started with Windows Admin Center 入门

>适用于：Windows Admin Center，Windows Admin Center 预览版

> [!Tip]
> 不熟悉 Windows Admin Center？
> [了解有关 Windows Admin Center 的更多信息](../understand/windows-admin-center.md)或[立即下载](https://aka.ms/windowsadmincenter)。

## <a name="windows-admin-center-installed-on-windows-10"></a>Windows 10 上安装 Windows Admin Center

> [!IMPORTANT]
> 必须是要在 Windows 10 上使用 Windows Admin Center 的本地管理员组的成员

### <a name="selecting-a-client-certificate"></a>选择客户端证书

首次打开 Windows Admin Center 在 Windows 10 中，请务必选择*Windows Admin Center 客户端*（否则将收到 HTTP 403 错误消息"无法获取到此页"） 的证书。

在 Microsoft Edge 中，出现此对话框的提示：
 
1. 单击**更多选择**

    ![](../media/launch-cert-1.png)

2. 选择标记的证书**Windows Admin Center 客户端**单击**确定**

    ![](../media/launch-cert-2.png)

3. 请确保**始终允许访问**处于选中状态，并单击**允许**

    ![](../media/launch-cert-3.png)

## <a name="connecting-to-managed-nodes-and-clusters"></a>连接到托管的节点和群集

完成 Windows Admin Center 安装后，可以添加服务器或群集来管理从主概述页。

 **将一台服务器或群集添加为被管理节点**

 1. 单击 **+ 添加**下**的所有连接**。

    ![](../media/launch/addserver0.png)

 2. 选择要添加的服务器、 故障转移群集或 Hyper-Converged 群集连接：
    
    ![](../media/launch/addserver1.png)

 3. 键入的服务器或群集管理和单击名称**提交**。 服务器或群集将添加到您在概述页上的连接列表。

    ![](../media/launch/addserver2.png)

   **-- OR --**

**大容量导入多台服务器**

 1. 上**添加服务器连接**页上，选择**导入服务器**选项卡。

    ![](../media/launch/import-servers.png)

 2. 单击**浏览**并选择一个文本文件，其中包含一个逗号，或新行分隔的你想要添加的服务器的 Fqdn 列表。

    **-- OR --**

**通过搜索 Active Directory 中添加服务器**

 1. 上**添加服务器连接**页上，选择**搜索 Active Directory**选项卡。

    ![](../media/launch/search-ad.png)

 2. 输入您的搜索条件，然后单击**搜索**。 支持通配符 （*）。

 3. 搜索完成后-选择一个或多个结果，可以选择添加标记，然后单击**添加**。

## <a name="authenticate-with-the-managed-node"></a>使用托管的节点进行身份验证 ##

Windows Admin Center 支持多种机制进行身份验证与托管节点。 单一登录是默认值。

**单一登录**

可以使用你当前的 Windows 凭据进行身份验证与托管节点。 这是默认值，并添加服务器时，Windows Admin Center 尝试登录。 

**单一登录时作为 Windows 服务器上的服务部署**

如果已在 Windows Server 上安装 Windows Admin Center，则为实现单一登录所需额外配置。  [配置委派的环境](..\configure\user-access-control.md)

**-- OR --**

**使用*管理方式*指定凭据**

下**的所有连接**，从列表中选择一台服务器，然后选择**管理方式**来指定将用于对被管理的节点进行身份验证的凭据：

![](../media/launch-use-6.png)

如果 Windows Admin Center 在服务模式下运行 Windows Server 上，但不是具有配置的 Kerberos 委派，必须重新输入你的 Windows 凭据：

![](../media/launch-use-7.png)

可能会将凭据应用到所有连接，请将其缓存为该特定浏览器会话。 如果你重新加载您的浏览器，必须重新输入您**管理方式**凭据。

**本地管理员密码解决方案 (LAPS)**

如果您的环境使用[LAPS](https://technet.microsoft.com/mt227395.aspx)，可以使用 LAPS 凭据进行身份验证与托管节点。 **如果使用这种情况下，请**[提供的反馈](http://aka.ms/WACFeedback)。

## <a name="using-tags-to-organize-your-connections"></a>使用标记来组织您的连接

可以使用标记来标识和连接列表中筛选相关的服务器。  这样，您可以查看连接列表中的服务器的子集。  这是特别有用，如果有多个连接。

### <a name="edit-tags"></a>编辑标记

* 所有连接列表中选择一台或多个服务器
* 下**的所有连接**，单击**编辑标记**

![](../media/launch/tags-5.png)

**编辑连接标记**窗格允许您修改、 添加或删除标记从你选择的连接：

* 若要将新标记添加到你选择的连接，请选择**将标记添加**并输入你想要使用的标记名称。

* 若要标记的现有标记名称与所选的连接，检查你想要应用的标记名称旁边的框。

* 若要从所有选定的连接中删除一个标记，请取消选中你想要删除的标记旁边的框。

* 如果标记应用于所选的连接的子集，该复选框会显示的中间状态。 可以单击相应的框以检查它，并将标记应用到所选的所有连接，或再次单击以取消选中它并从所有选定的连接中删除该标记。

![](../media/launch/tags-6.png)

### <a name="filter-connections-by-tag"></a>按标记筛选连接

后标记已添加到一个或多个服务器连接，可以在连接列表中，查看的标记，并按标记筛选连接列表。

* 若要按标记进行筛选，选择搜索框旁边的筛选器图标。
![](../media/launch/tags-7.png)
* 可以选择"，"和"或"not"以修改所选标记的筛选器行为。
![](../media/launch/tags-8.png)

## <a name="use-powershell-to-import-or-export-your-connections-with-tags"></a>使用 PowerShell 导入或导出您的连接 （使用标记）

> 适用于：Windows Admin Center 预览版

Windows Admin Center 预览版包括一个 PowerShell 模块导入或导出连接列表。

```powershell
# Load the module
Import-Module "$env:ProgramFiles\windows admin center\PowerShell\Modules\ConnectionTools"
# Available cmdlets: Export-Connection, Import-Connection

# Export connections (including tags) to .csv files
Export-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv"
# Import connections (including tags) from .csv files
Import-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv"
```

### <a name="csv-file-format-for-importing-connections"></a>CSV 文件格式导入连接

CSV 文件的格式启动包含四个标题```"name","type","tags","groupId"```后, 跟新行上的每个连接。

**名称**是连接的 FQDN

**类型**是连接类型。 针对 Windows Admin Center 中包含的默认连接，将使用以下项之一：

| 连接类型 | 连接字符串 |
|------|-------------------------------|
| Windows Server | msft.sme.connection-type.server |
| Windows 10 电脑 | msft.sme.connection-type.windows-client |
| 故障转移群集 | msft.sme.connection-type.cluster |
| 超聚合群集 | msft.sme.connection-type.hyper-converged-cluster |

**标记**是竖线分隔。

**groupId**用于共享连接。 使用值```global```中此列，从而使此共享的连接。

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

使用下面的脚本导出中的已保存的连接[RDCman](https://blogs.technet.microsoft.com/rmilne/2014/11/19/remote-desktop-connection-manager-download-rdcman-2-7/)到文件。 你可以然后将文件导入 Windows Admin Center 维护 RDCMan 分组层次结构使用标记。 试试看 ！

1. 复制并粘贴到 PowerShell 会话的以下代码：

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

2. 若要创建。CSV 文件中，运行以下命令：

   ```powershell
   RdgToWacCsv -RDGfilepath "path\to\myRDCManfile.rdg"
   ```

3. 导入生成。连接列表中的标记将表示为 Windows Admin Center 中的 CSV 文件和所有 RDCMan 分组层次结构。 有关详细信息，请参阅[使用 PowerShell 导入或导出 （加上标记） 连接](#use-powershell-to-import-or-export-your-connections-with-tags)。

## <a name="view-powershell-scripts-used-in-windows-admin-center"></a>查看 Windows Admin Center 中使用的 PowerShell 脚本

连接到服务器、 群集或 PC 后，可以查看 PowerShell 脚本这一功能可用的 UI 操作 Windows Admin Center 中。 从在工具中，单击顶部的应用程序栏中的 PowerShell 图标。 从下拉列表中，导航到相应的 PowerShell 脚本中选择所需的命令。

![](../media/launch/showscript.png)