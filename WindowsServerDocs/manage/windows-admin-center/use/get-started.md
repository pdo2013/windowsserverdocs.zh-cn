---
title: 开始使用 Windows Admin Center
description: 开始使用 Windows Admin Center
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 02/15/2019
ms.openlocfilehash: f4fd9f69e75ed80bbdb345b4041c2337c65ec2e6
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296649"
---
# 开始使用 Windows Admin Center

>适用于：Windows Admin Center、Windows Admin Center 预览版

> [!Tip]
> 不熟悉 Windows Admin Center？
> [了解有关 Windows Admin Center 的更多信息](../understand/windows-admin-center.md)或[立即下载](https://aka.ms/windowsadmincenter)。

## Windows 10 上安装 Windows Admin Center

> [!IMPORTANT]
> 你必须是 Windows 10 上使用 Windows Admin Center 的本地管理员组的成员

### 选择客户端证书

首次打开 Windows 10 上的 Windows Admin Center 确保选择 （否则你将收到 HTTP 403 错误消息指出"无法访问此页"） 的*Windows Admin Center 客户端*证书。

在 Microsoft Edge 中，此对话框出现的提示：
 
1. 单击**更多选项**

    ![](../media/launch-cert-1.png)

2. 选择标记为**Windows Admin Center 客户端**证书，并单击**确定**

    ![](../media/launch-cert-2.png)

3. 请确保选中**始终允许访问**并单击**允许**

    ![](../media/launch-cert-3.png)

## 连接到托管的节点和群集

完成了 Windows Admin Center 安装后，你可以添加服务器或群集管理在主概述页中。

 **作为托管节点添加单个服务器或群集**

 1. 单击下**的所有连接**的 **+ 添加**。

    ![](../media/launch/addserver0.png)

 2. 选择要添加服务器、 故障转移群集或超融合群集连接：
    
    ![](../media/launch/addserver1.png)

 3. 键入服务器或群集管理并单击**提交**的名称。 服务器或群集将添加到你在概述页面上的连接列表。

    ![](../media/launch/addserver2.png)

   **-- 或者 --**

**批量导入多个服务器**

 1. 在**添加服务器连接**页面上选择**导入的服务器**选项卡。

    ![](../media/launch/import-servers.png)

 2. 单击**浏览**并选择包含逗号或换行符分隔，你想要添加的服务器的 Fqdn 列表的文本文件。

    **-- 或者 --**

**通过搜索 Active Directory 添加服务器**

 1. 在**添加服务器连接**页面上选择**搜索 Active Directory**选项卡。

    ![](../media/launch/search-ad.png)

 2. 输入搜索条件并单击**搜索**。 支持通配符 （*）。

 3. 搜索完成后的选择一个或多个结果、 （可选） 添加标记，并单击**添加**。

## 使用托管节点进行身份验证 ##

Windows Admin Center 支持几种机制，用于托管节点中进行身份验证。 单一登录是默认设置。

**单一登录**

你当前的 Windows 凭据可用于使用托管节点进行身份验证。 这是默认设置，并添加服务器时，Windows Admin Center 尝试登录。 

**单一登录时作为 Windows Server 上的服务部署**

如果你已在 Windows Server 上安装 Windows Admin Center，不需要的单一登录其他配置。  [配置你的环境进行委派](..\configure\user-access-control.md)

**-- 或者 --**

**使用*管理方式*指定凭据**

在**所有连接**，从列表中选择服务器和选择**管理方式**来指定将用于到托管节点进行身份验证的凭据：

![](../media/launch-use-6.png)

如果 Windows Admin Center 在服务模式下运行 Windows Server 上，但不是具有配置的 Kerberos 委派，必须重新输入你的 Windows 凭据：

![](../media/launch-use-7.png)

你可能适用于所有连接，这将为该特定浏览器会话缓存它们的凭据。 如果你重新加载浏览器，你必须重新输入你**管理方式**的凭据。

**本地管理员密码解决方案 (使用 LAPS)**

如果你的环境使用[LAPS](https://technet.microsoft.com/mt227395.aspx)，可以使用 LAPS 凭据进行身份验证的托管的节点。 **如果你使用此方案中，请**[提供反馈](http://aka.ms/WACFeedback)。

## 使用标记组织你连接

你可以使用标记来识别和筛选相关的服务器连接列表中。  这样，你可以查看你的服务器连接列表中的一个子集。  这是特别有用，如果你有多个连接。

### 编辑标记

* 所有的连接列表中选择一台或多个服务器
* 在**所有连接**下，单击**编辑标记**

![](../media/launch/tags-5.png)

**编辑连接标记**窗格可以修改、 添加或删除从你选择的连接的标记：

* 若要将新的标记添加到你选择的连接，选择**添加标记**，并输入你想要使用的标记名称。

* 若要标记现有的标记名称与所选的连接，请检查你想要应用的标记名称旁边的框。

* 若要从所有选定连接删除标记，请取消选中你想要删除的标记旁边的框。

* 如果某个标记应用于所选的连接的子集，是处于中间状态显示复选框。 你可以单击该框，以检查它，并将标记应用到所有选定的连接，或再次单击以取消选中它并删除所有选定的连接的标记。

![](../media/launch/tags-6.png)

### 通过标记筛选器连接

标记添加到一个或多个服务器连接后，可以在连接列表中，查看标记并按标记筛选连接列表。

* 若要按标记筛选，选择搜索框旁边的筛选器图标。
![](../media/launch/tags-7.png)
* 你可以选择"，"和"或"不"来修改选定的标记的筛选器行为。
![](../media/launch/tags-8.png)

## 使用 PowerShell 来导入或导出你连接 （通过标记）

> 适用于： Windows Admin Center 预览版

Windows Admin Center 预览版包括的 PowerShell 模块导入或导出你的连接列表。

```powershell
# Load the module
Import-Module "$env:ProgramFiles\windows admin center\PowerShell\Modules\ConnectionTools"
# Available cmdlets: Export-Connection, Import-Connection

# Export connections (including tags) to .csv files
Export-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv"
# Import connections (including tags) from .csv files
Import-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv"
```

### 导入连接的 CSV 文件格式

CSV 文件的格式开头四个标题```"name","type","tags","groupId"```后, 跟的新行上每个连接。

**名称**是连接的 FQDN

**类型**是连接类型。 对于使用 Windows Admin Center 包含默认连接，你将使用以下值之一：

| 连接类型 | 连接字符串 |
|------|-------------------------------|
| Windows Server | msft.sme.connection type.server |
| Windows 10 电脑 | msft.sme.connection type.windows 的客户端 |
| 故障转移群集 | msft.sme.connection type.cluster |
| 超融合群集 | msft.sme.connection 端 type.hyper 聚合的群集 |

**标记**为管道分隔。

**groupId**用于共享的连接。 使用值```global```若要将此共享的连接此列中。

### 用于导入连接的示例 CSV 文件

```
"name","type","tags","groupId"
"myServer.contoso.com","msft.sme.connection-type.server","hyperv"
"myDesktop.contoso.com","msft.sme.connection-type.windows-client","hyperv"
"teamcluster.contoso.com","msft.sme.connection-type.cluster","legacyCluster|WS2016","global"
"myHCIcluster.contoso.com,"msft.sme.connection-type.hyper-converged-cluster","myHCIcluster|hyperv|JIT|WS2019"
"teamclusterNode.contoso.com","msft.sme.connection-type.server","legacyCluster|WS2016","global"
"myHCIclusterNode.contoso.com","msft.sme.connection-type.server","myHCIcluster|hyperv|JIT|WS2019"
```

## 导入 RDCman 连接

下面的脚本用于导出到文件中[RDCman](https://blogs.technet.microsoft.com/rmilne/2014/11/19/remote-desktop-connection-manager-download-rdcman-2-7/)已保存的连接。 你可以然后文件导入到 Windows Admin Center，维护使用标记你 RDCMan 分组层次结构。 尝试一下 ！

1. 复制并粘贴到你的 PowerShell 会话中的以下代码：

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

2. 若要创建。CSV 文件，请运行以下命令：

   ```powershell
   RdgToWacCsv -RDGfilepath "path\to\myRDCManfile.rdg"
   ```

3. 导入生成。Windows Admin center 中的 CSV 文件，并且所有 RDCMan 分组层次结构中将由连接列表中的标记表示。 有关详细信息，请参阅[使用 PowerShell 导入或导出你连接 （通过标记）](#use-powershell-to-import-or-export-your-connections-with-tags)。

## 查看使用 Windows Admin Center 中的 PowerShell 脚本

一旦你已连接到服务器、 群集或电脑，你可以查看 PowerShell 脚本的电源可用的 UI 操作 Windows Admin Center 中。 从内工具中，单击顶部的应用程序栏中的 PowerShell 图标。 从下拉列表中导航到相应的 PowerShell 脚本选择感兴趣的命令。

![](../media/launch/showscript.png)