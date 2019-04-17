---
title: 安装和管理扩展
description: 安装和管理 Windows Admin Center (Project Honolulu) 中的扩展
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: c775dd5a3011115bbb031c0b9e4e24a8911d378e
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296699"
---
# 安装和管理扩展

>适用于：Windows Admin Center、Windows Admin Center 预览版

Windows Admin Center 构建为一个可扩展的平台，其中每个连接类型和工具是扩展，你可以安装、 卸载和单独更新。 你可以搜索 Microsoft 和其他开发人员发布的新扩展和安装单独更新而无需更新整个 Windows Admin Center 安装。 此外可以配置单独的 NuGet 源或文件共享和分配扩展以在组织内的内部使用。

## 安装扩展

Windows Admin Center 将从指定的 NuGet 源显示可用的扩展。 默认情况下，Windows Admin Center 指向源 Microsoft 官方 NuGet 托管由 Microsoft 和其他开发人员发布的扩展。

1. 单击右上角 > 在左侧窗格中的**设置**按钮，然后单击**扩展**。 
2. **可用的扩展**选项卡将列出源上可用于安装的扩展。
3. 单击扩展**的详细信息**窗格中查看扩展说明、 版本、 发布者和其他信息。
4. 单击**安装**要安装的扩展。 如果该网关必须运行在提升模式下进行此更改，你将看到 UAC 提升权限提示。 安装完成后，将自动刷新浏览器和 Windows Admin Center 将重新加载与新安装的扩展。 如果你正在安装的扩展是之前已安装的扩展的更新，你可以单击**更新到最新**按钮以安装更新。 你还可以转到**已安装的扩展**选项卡查看已安装的扩展和更新在**状态**列中可用。

## 来自不同源安装扩展

Windows Admin Center 支持多个源，可以查看和管理来自多个源一次的程序包。 任何 NuGet 源支持的 NuGet V2 Api 或文件共享可以安装从扩展添加到 Windows Admin Center。

1. 单击右上角 > 在左侧窗格中的**设置**按钮，然后单击**扩展**。
2. 在右窗格中，单击**源**选项卡。
3. 单击**添加**按钮以添加另一个源。 有关 NuGet 订阅源，请输入源的 URL NuGet V2。 NuGet 源提供程序或管理员应能够提供的 URL 信息。 对于文件共享，输入该文件的完整路径中的扩展包文件 (.nupkg) 存储的共享。
4. 单击**添加**。 如果该网关必须运行在提升模式下进行此更改，你将看到 UAC 提升权限提示。

**可用的扩展**列表将显示来自所有已注册的订阅源的扩展。 你可以查看每个扩展是从使用的**程序包源**列的源。

## 卸载扩展

你可以卸载以前安装，任何扩展或甚至卸载已预安装 Windows Admin Center 安装的一部分的任何工具。

1. 单击右上角 > 在左侧窗格中的**设置**按钮，然后单击**扩展**。 
2. 单击**已安装的扩展**选项卡以查看所有已安装的扩展。
3. 选择扩展要卸载，然后单击**卸载**。

卸载后完成，将自动刷新浏览器和 Windows Admin Center 将删除扩展名重新加载。 如果卸载已预安装 Windows Admin Center 的一部分的工具，该工具将提供用于在**可用的扩展**选项卡中重新安装。

## 在没有 internet 连接的计算机上安装扩展

如果未连接到 internet 或在代理的后面的计算机上安装 Windows Admin Center，则它可能无法访问并从 Windows Admin Center 源安装扩展。 你可以手动或使用 PowerShell 脚本，下载扩展包并配置 Windows Admin Center 从文件共享或本地驱动器中获取软件包。

### 手动下载扩展包

1. 另一台计算机上具有 internet 连接，打开 web 浏览器并导航到以下 URL:[https://msft-sme.myget.org/gallery/windows-admin-center-feed](https://msft-sme.myget.org/gallery/windows-admin-center-feed) 

  * 你可能需要在 msft sme.myget.org 和登录以查看扩展包上创建帐户。

2. 单击你想要安装，以查看包的详细信息页面的包的名称。
3. 单击程序包详细信息页面右侧窗格中的**下载**链接并下载.nupkg 文件的扩展。
4. 对于你想要下载的所有程序包重复步骤 2 和 3。
5. 将包文件复制到可从安装 Windows Admin Center 的计算机访问的文件共享或计算机的本地磁盘。
6. [请按照说明安装来自不同源的扩展](#installing-extensions-from-a-different-feed)。

### 下载程序包使用 PowerShell 脚本

有许多脚本可用 internet 进行下载源 NuGet NuGet 程序包。 我们将使用[脚本提供 Jon Galloway](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell)，在 Microsoft 高级项目经理。

1. 如[博客文章](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell)中所述，将脚本安装为 NuGet 程序包或复制并粘贴到 PowerShell ISE 的脚本。
2. 编辑对你 NuGet 脚本的第一行源的 v2 URL。 如果要将下载源的官方 Windows Admin Center 的程序包，使用下面的 URL。

```powershell
$feedUrlBase = "https://aka.ms/sme-extension-feed"
```

3. 运行脚本，它也将到以下本地文件夹下载来自源的所有 NuGet 程序包： %USERPROFILE%\Documents\NuGetLocal
4. [请按照说明安装来自不同源的扩展](#installing-extensions-from-a-different-feed)。

## 管理扩展使用 PowerShell

>适用于：Windows Admin Center、Windows Admin Center 预览版

Windows Admin Center 预览版包括的 PowerShell 模块管理网关扩展。

```powershell
# Add the module to the current session
Import-Module "$env:ProgramFiles\windows admin center\PowerShell\Modules\ExtensionTools"
# Available cmdlets: Get-Feed, Add-Feed, Remove-Feed, Get-Extension, Install-Extension, Uninstall-Extension, Update-Extension

# List feeds
Get-Feed "https://wac.contoso.com"

# Add a new extension feed
Add-Feed -GatewayEndpoint "https://wac.contoso.com" -Feed "\\WAC\our-private-extensions"

# Remove an extension feed
Remove-Feed -GatewayEndpoint "https://wac.contoso.com" -Feed "\\WAC\our-private-extensions"

# List all extensions
Get-Extension "https://wac.contoso.com"

# Install an extension (locate the latest version from all feeds and install it)
Install-Extension -GatewayEndpoint "https://wac.contoso.com" "msft.sme.containers"

# Install an extension (latest version from a specific feed, if the feed is not present, it will be added)
Install-Extension -GatewayEndpoint "https://wac.contoso.com" "msft.sme.containers" -Feed "https://aka.ms/sme-extension-feed"

# Install an extension (install a specific version)
Install-Extension "https://wac.contoso.com" "msft.sme.certificate-manager" "0.133.0"

# Uninstall-Extension
Uninstall-Extension "https://wac.contoso.com" "msft.sme.containers"

# Update-Extension
Update-Extension "https://wac.contoso.com" "msft.sme.containers"
```

### [了解有关生成使用 Windows Admin Center SDK 扩展详细信息](../extend/extensibility-overview.md)。