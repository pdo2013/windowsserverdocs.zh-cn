---
title: 安装和管理扩展
description: 安装和管理 Windows Admin Center (项目 Honolulu) 中的扩展
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 9038fd480ed105aed3949b0c48dffc7eab94f970
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445895"
---
# <a name="install-and-manage-extensions"></a>安装和管理扩展

>适用于：Windows Admin Center，Windows Admin Center 预览版

其中每个连接类型和工具是一个扩展，您可以安装、 卸载和更新单独一个可扩展的平台生成为 Windows Admin Center。 可以搜索发布的 Microsoft 和其他开发人员的新扩展并安装和无需更新整个 Windows Admin Center 安装单独更新。 此外可以配置单独的 NuGet 源或文件共享并将扩展在内部使用您的组织内分发。

## <a name="installing-an-extension"></a>安装扩展

Windows Admin Center 将从指定的 NuGet 源显示可用的扩展。 默认情况下，Windows Admin Center 指向 Microsoft 官方 NuGet 源托管由 Microsoft 和其他开发人员发布的扩展。

1. 单击**设置**在右上角的按钮 > 在左窗格中，单击**扩展**。 
2. **可用扩展**选项卡将列出可供安装源上的扩展。
3. 单击要查看扩展说明、 版本、 发布服务器和中的其他信息的扩展**详细信息**窗格。
4. 单击**安装**安装某个扩展。 如果网关必须运行在提升模式下，若要进行此更改，则将显示与 UAC 提升提示。 安装完成后，将自动刷新你的浏览器和 Windows Admin Center 将重新加载已安装新扩展。 如果您要尝试扩展安装是对以前已安装扩展的更新，可以单击**更新到最新**按钮以安装更新。 此外可转到**已安装的扩展**选项卡上查看已安装扩展和，请参阅如果更新现已推出**状态**列。

## <a name="installing-extensions-from-a-different-feed"></a>从不同的源安装扩展

Windows Admin Center 支持多个馈送，您可以查看和管理包从一次的多个数据源。 任何 NuGet 源支持的 NuGet V2 Api 或文件共享可用于安装的扩展添加到 Windows Admin Center。

1. 单击**设置**在右上角的按钮 > 在左窗格中，单击**扩展**。
2. 在右窗格中，单击**馈送**选项卡。
3. 单击**添加**按钮以添加另一个源。 NuGet 源，请输入源 URL NuGet V2。 NuGet 源提供程序或管理员应该能够提供的 URL 信息。 对于文件共享，请输入该文件的完整路径的扩展包文件 (.nupkg) 存储共享。
4. 单击**添加**。 如果网关必须运行在提升模式下，若要进行此更改，则将显示与 UAC 提升提示。

**可用扩展**列表将显示所有已注册的源的扩展。 可以检查每个扩展是从使用哪些源**包源**列。

## <a name="uninstalling-an-extension"></a>卸载扩展

可以卸载任何扩展之前已安装，或甚至卸载已预装 Windows Admin Center 安装的任何工具。

1. 单击**设置**在右上角的按钮 > 在左窗格中，单击**扩展**。 
2. 单击**已安装的扩展**选项卡以查看所有已安装的扩展。
3. 选择一个扩展卸载，然后单击**卸载**。

卸载后已完成，请将自动刷新你的浏览器并将重新加载 Windows Admin Center 与扩展名已移除。 如果你卸载 Windows Admin Center 的一部分预安装的工具，该工具都可用于在重新安装**可用扩展**选项卡。

## <a name="installing-extensions-on-a-computer-without-internet-connectivity"></a>在没有 internet 连接的计算机上安装扩展

如果未连接到 internet 或者位于代理的计算机上安装 Windows Admin Center，则它可能无法访问，并从 Windows Admin Center 源安装的扩展。 您可以手动或使用 PowerShell 脚本，下载扩展包和配置 Windows Admin Center 从文件共享或本地驱动器中检索包。

### <a name="manually-downloading-extension-packages"></a>手动下载扩展包

1. 在已建立 internet 连接的另一台计算机，打开 web 浏览器并导航到以下 URL: [https://msft-sme.myget.org/gallery/windows-admin-center-feed](https://msft-sme.myget.org/gallery/windows-admin-center-feed) 

   * 您可能需要 msft sme.myget.org 和登录名可以查看扩展包上创建一个帐户。

2. 单击你想要安装若要查看包详细信息页的包的名称。
3. 单击**下载**包详细信息页的右侧窗格中的链接并下载该扩展的.nupkg 文件。
4. 你想要下载的所有包重复步骤 2 和 3。
5. 将包文件复制到可以从 Windows Admin Center 安装所在的计算机访问的文件共享或本地计算机的磁盘。
6. [按照说明进行操作以从不同的源安装扩展](#installing-extensions-from-a-different-feed)。

### <a name="downloading-packages-with-a-powershell-script"></a>下载 PowerShell 脚本的包

有许多脚本可从 NuGet 源下载 NuGet 包在 Internet 上。 我们将使用[Jon galloway 提供脚本](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell)，microsoft 高级项目经理。

1. 如中所述[博客文章](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell)、 作为 NuGet 包，安装脚本或脚本复制并粘贴到 PowerShell ISE。
2. 编辑源脚本保存到 NuGet 的第一行的第 2 版 URL。 如果要下载 Windows Admin Center 正式从包源，请使用以下 URL。

```powershell
$feedUrlBase = "https://aka.ms/sme-extension-feed"
```

3. 运行脚本，它将下载所有 NuGet 包从数据源到以下本地文件夹： %USERPROFILE%\Documents\NuGetLocal
4. [按照说明进行操作以从不同的源安装扩展](#installing-extensions-from-a-different-feed)。

## <a name="manage-extensions-with-powershell"></a>管理使用 PowerShell 扩展

>适用于：Windows Admin Center，Windows Admin Center 预览版

Windows Admin Center 预览版包括一个 PowerShell 模块，以管理网关扩展。

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

### <a name="learn-more-about-building-an-extension-with-the-windows-admin-center-sdkextendextensibility-overviewmd"></a>[详细了解如何生成使用 Windows Admin Center SDK 扩展](../extend/extensibility-overview.md)。