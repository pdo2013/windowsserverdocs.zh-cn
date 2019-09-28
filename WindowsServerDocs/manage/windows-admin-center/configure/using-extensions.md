---
title: 安装和管理扩展
description: 在 Windows 管理中心中安装和管理扩展（Project Honolulu）
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: d49e25591c705afa217b2332ee48eb42c5c2f7ab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357239"
---
# <a name="install-and-manage-extensions"></a>安装和管理扩展

>适用于：Windows Admin Center、Windows Admin Center 预览版

Windows 管理中心构建为可扩展的平台，其中的每个连接类型和工具都是可单独安装、卸载和更新的扩展。 你可以搜索由 Microsoft 和其他开发人员发布的新扩展，并单独安装和更新它们，而无需更新整个 Windows 管理中心安装。 你还可以配置单独的 NuGet 源或文件共享，并分发要在组织内部使用的扩展。

## <a name="installing-an-extension"></a>安装扩展

Windows 管理中心将显示指定的 NuGet 源提供的扩展。 默认情况下，Windows 管理中心会指向承载由 Microsoft 和其他开发人员发布的扩展的 Microsoft 官方 NuGet 源。

1. 单击左侧窗格中右上方 > 的 "**设置**" 按钮，然后单击 "**扩展**"。 
2. "**可用扩展**" 选项卡将列出可供安装的源上的扩展。
3. 单击某个扩展可在**详细信息**窗格中查看扩展说明、版本、发布者和其他信息。
4. 单击 "**安装**" 以安装扩展。 如果网关必须在提升模式下运行才能进行此更改，则会显示 UAC 提升提示。 安装完成后，浏览器将自动刷新，并将在安装了新扩展的情况下重新加载 Windows 管理中心。 如果尝试安装的扩展是对以前安装的扩展的更新，则可以单击 "**更新到最新版本**" 按钮来安装更新。 你还可以访问 "**已安装的扩展**" 选项卡以查看已安装的扩展，并查看 "**状态**" 列中是否有更新。

## <a name="installing-extensions-from-a-different-feed"></a>从其他源安装扩展

Windows 管理中心支持多个源，你可以一次查看和管理多个源中的包。 可以将支持 NuGet V2 Api 或文件共享的任何 NuGet 源添加到用于安装的扩展的 Windows 管理中心。

1. 单击左侧窗格中右上方 > 的 "**设置**" 按钮，然后单击 "**扩展**"。
2. 在右侧窗格中，单击 "**源**" 选项卡。
3. 单击 "**添加**" 按钮添加另一个源。 对于 NuGet 源，请输入 NuGet V2 源 URL。 NuGet 源提供程序或管理员应能够提供 URL 信息。 对于文件共享，请输入存储扩展包文件（. nupkg）的文件共享的完整路径。
4. 单击**添加**。 如果网关必须在提升模式下运行才能进行此更改，则会显示 UAC 提升提示。

"**可用扩展**" 列表将显示所有已注册源的扩展。 您可以使用 "**包源**" 列检查每个扩展的源。

## <a name="uninstalling-an-extension"></a>卸载扩展

您可以卸载以前安装的任何扩展，甚至卸载在 Windows 管理中心安装过程中预安装的任何工具。

1. 单击左侧窗格中右上方 > 的 "**设置**" 按钮，然后单击 "**扩展**"。 
2. 单击 "**已安装的扩展**" 选项卡以查看所有已安装的扩展。
3. 选择要卸载的扩展，并单击 "**卸载**"。

卸载完成后，浏览器将自动刷新，Windows 管理中心会重新加载，并删除该扩展。 如果卸载的工具是作为 Windows 管理中心的一部分预安装的，则该工具将可在可用的 "**扩展**" 选项卡上重新安装。

## <a name="installing-extensions-on-a-computer-without-internet-connectivity"></a>在没有 internet 连接的计算机上安装扩展

如果在未连接到 internet 或者位于代理后面的计算机上安装了 Windows 管理中心，则它可能无法从 Windows 管理中心源访问和安装扩展。 你可以手动或通过 PowerShell 脚本下载扩展包，并配置 Windows 管理中心以从文件共享或本地驱动器检索包。

### <a name="manually-downloading-extension-packages"></a>手动下载扩展包

1. 在具有 internet 连接的另一台计算机上，打开 web 浏览器并导航到以下 URL： [https://msft-sme.myget.org/gallery/windows-admin-center-feed](https://msft-sme.myget.org/gallery/windows-admin-center-feed) 

   * 你可能需要在 msft-sme.myget.org 上创建一个帐户，并登录以查看扩展包。

2. 单击要安装的包的名称，以查看包详细信息页。
3. 在包详细信息页的右侧窗格中单击 "**下载**" 链接，然后下载该扩展的 nupkg 文件。
4. 对于要下载的所有包，请重复步骤2和3。
5. 将包文件复制到可从安装 Windows 管理中心的计算机上访问的文件共享，或复制到计算机的本地磁盘。
6. [按照说明安装不同源中的扩展](#installing-extensions-from-a-different-feed)。

### <a name="downloading-packages-with-a-powershell-script"></a>使用 PowerShell 脚本下载包

Internet 上提供了许多可用于从 NuGet 源下载 NuGet 包的脚本。 我们将使用[吴建 Galloway](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell)（Microsoft 的高级项目经理）提供的脚本。

1. 如[博客文章](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell)中所述，将脚本安装为 NuGet 包，或将脚本复制并粘贴到 PowerShell ISE。
2. 将脚本的第一行编辑到 NuGet 源的 v2 URL。 如果要从 Windows 管理中心官方源下载包，请使用下面的 URL。

```powershell
$feedUrlBase = "https://aka.ms/sme-extension-feed"
```

3. 运行脚本，并将源中的所有 NuGet 包下载到以下本地文件夹：%USERPROFILE%\Documents\NuGetLocal
4. [按照说明安装不同源中的扩展](#installing-extensions-from-a-different-feed)。

## <a name="manage-extensions-with-powershell"></a>通过 PowerShell 管理扩展

>适用于：Windows Admin Center、Windows Admin Center 预览版

Windows 管理中心预览版包含用于管理网关扩展的 PowerShell 模块。

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

### <a name="learn-more-about-building-an-extension-with-the-windows-admin-center-sdkextendextensibility-overviewmd"></a>[详细了解如何使用 Windows 管理中心 SDK 生成扩展](../extend/extensibility-overview.md)。