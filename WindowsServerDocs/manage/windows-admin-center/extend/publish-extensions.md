---
title: 发布 Windows 管理中心的扩展
description: 发布 Windows 管理中心的扩展（Project Honolulu）
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 24beb287aa35757e1f8057920e8fd95828baf83b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385200"
---
# <a name="publishing-extensions"></a>发布扩展

>适用于：Windows Admin Center、Windows Admin Center 预览版

开发扩展后，需要将其发布，使其可供其他人使用。 根据你的受众和发布的目的，我们将在下面介绍几个选项，以及发布的步骤和要求。

## <a name="publishing-options"></a>发布选项

Windows 管理中心支持的可配置包源有三个主要选项：
* Microsoft 的公共 Windows 管理中心 NuGet 源
* 你自己的专用 NuGet 源
* 本地或网络文件共享

### <a name="publishing-to-the-windows-admin-center-extension-feed"></a>发布到 Windows 管理中心扩展源

默认情况下，Windows 管理中心连接到 Microsoft 的 Windows 管理中心产品团队维护的 NuGet 源。 Microsoft 开发的新扩展的早期预览版本可能会发布到此源，并可供 Windows 管理中心用户使用。 计划公开生成和发布扩展的外部开发人员还可以提交发布到此源[的请求](#publishing-your-extension-to-the-windows-admin-center-feed)。

### <a name="publishing-to-a-different-nuget-feed"></a>发布到不同的 NuGet 源

您还可以创建自己的 NuGet 源，以使用[用于设置专用源或使用 NuGet 托管服务的多种不同选项](https://docs.microsoft.com/nuget/hosting-packages/overview)之一发布扩展。 NuGet 源必须支持 NuGet v2 API。 由于 Windows 管理中心当前不支持源身份验证，因此需要将源配置为允许任何人进行读访问。

### <a name="publishing-to-a-file-share"></a>发布到文件共享

若要将扩展访问权限限制给组织或限制的人员组，可以使用 SMB 文件共享作为扩展源。 在这种情况下，将应用文件共享和文件夹权限以允许访问源。

## <a name="preparing-your-extension-for-release"></a>准备要发布的扩展

请确保阅读并考虑以下开发主题：

- [控制工具的可见性](guides/dynamic-tool-display.md)
- [字符串和本地化](guides/strings-localization.md)

### <a name="consider-releasing-as-a-preview-release"></a>考虑发布为预览版

如果出于评估目的发布扩展的预览版本，则建议你执行以下操作：

- 将 "（预览版）" 追加到 nuspec 文件中扩展插件的末尾
- 说明 nuspec 文件中扩展说明的限制

## <a name="creating-an-extension-package"></a>创建扩展包

Windows 管理中心使用 NuGet 包和源来分发和下载扩展。  为了使你的包能够装运，你将需要生成包含插件和扩展的 NuGet 包。  单个包可以同时包含 UI 扩展插件和网关插件，以下部分将引导你完成此过程。

### <a name="1-build-your-extension"></a>1.构建扩展

一旦准备好开始打包扩展，请在文件系统上创建一个新目录，打开控制台，并将 CD 放入其中。  这将是一个根目录，我们将使用该目录来包含构成包的所有 nuspec 和内容目录。  我们会在本文档的持续时间内将此文件夹引用为 "NuGet 包"。

#### <a name="ui-extensions"></a>UI 扩展

若要开始收集 UI 扩展所需的所有内容，请在工具中运行 "gulp 生成"，并确保生成成功。  此过程会将所有组件一起打包到一个名为 "捆绑" 的文件夹中，该文件夹位于扩展的根目录中（位于 src 目录的同一级别）。  将此目录及其所有内容复制到 "NuGet 包" 文件夹中。

#### <a name="gateway-plugins"></a>网关插件

使用您的构建基础结构（这可能非常简单，只是打开 Visual Studio 并单击 "生成" 按钮），然后编译并生成插件。  打开您的生成输出目录，并复制代表您的插件的 Dll，并将其放在名为 "Package" 的 "NuGet 包" 目录中的新文件夹中。  不需要复制 FeatureInterface dll，只需复制代表代码的 Dll。

### <a name="2-create-the-nuspec-file"></a>2.创建 nuspec 文件

若要创建 NuGet 包，需要首先创建一个 nuspec 文件。 Nuspec 文件是包含 NuGet 包元数据的 XML 清单。 此清单既用于生成包, 也用于向使用者提供信息。  将此文件放置在 "NuGet 包" 文件夹的根目录下。

下面是 nuspec 文件以及必需或推荐属性的列表。 有关完整的架构，请参阅[nuspec 引用](https://docs.microsoft.com/nuget/reference/nuspec)。 将 nuspec 文件保存到项目的根文件夹中，文件名为你选择的文件名。

> [!IMPORTANT]
> Nuspec ```<id>```文件中的值需要与项目```manifest.json```文件中的```"name"```值匹配，否则，已发布的扩展将不会在 Windows 管理中心中成功加载。

```xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2011/08/nuspec.xsd">
  <metadata>
    <packageTypes>
      <packageType name="WindowsAdminCenterExtension" />
    </packageTypes>  
    <id>contoso.project.extension</id>
    <version>1.0.0</version>
    <title>Contoso Hello Extension</title>
    <authors>Contoso</authors>
    <owners>Contoso</owners>
    <requireLicenseAcceptance>false</requireLicenseAcceptance>
    <projectUrl>https://msft-sme.myget.org/feed/windows-admin-center-feed/package/nuget/contoso.sme.hello-extension</projectUrl>
    <licenseUrl>http://YourLicenseLink</licenseUrl>
    <iconUrl>http://YourLogoLink</iconUrl>
    <description>Hello World extension by Contoso</description>
    <copyright>(c) Contoso. All rights reserved.</copyright> 
    <tags></tags>
  </metadata>
  <files>
    <file src="bundle\**\*.*" target="ux" />
    <file src="package\**\*.*" target="gateway" />
  </files>
</package>
```

#### <a name="required-or-recommended-properties"></a>必需或推荐的属性

| 属性名 | 必需/建议 | 描述 |
| ---- | ---- | ---- |
| PackageType | 必需 | 使用 "WindowsAdminCenterExtension"，它是为 Windows 管理中心扩展定义的 NuGet 包类型。 |
| id | 必需 | 源中唯一的包标识符。 此值需要与项目的清单 json 文件中的 "名称" 值匹配。  有关指南, 请参阅[选择唯一的包标识符](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number)。 |
| title | 需要将其发布到 Windows 管理中心源 | Windows 管理中心扩展管理器中显示的包的友好名称。 |
| version | 必需 | 扩展版本。 建议使用[语义版本控制（SemVer 约定）](http://semver.org/spec/v1.0.0.html) ，但这不是必需的。 |
| 人员 | 必需 | 如果代表你的公司发布，请使用你的公司名称。 |
| description | 必需 | 提供扩展功能的说明。 |
| iconUrl | 在发布到 Windows 管理中心源时建议 | 要在扩展管理器中显示的图标的 URL。 |
| projectUrl | 需要将其发布到 Windows 管理中心源 | 指向扩展网站的 URL。 如果没有单独的网站，请使用 NuGet 源上包网页的 URL。 |
| licenseUrl | 需要将其发布到 Windows 管理中心源 | 指向扩展的最终用户许可协议的 URL。 |
| files | 必需 | 这两个设置设置 Windows 管理中心需要用于 UI 扩展和网关插件的文件夹结构。 |

### <a name="3-build-the-extension-nuget-package"></a>3.构建扩展 NuGet 包

使用前面创建的 nuspec 文件，你现在可以创建 NuGet 包 nupkg 文件，你可以将该文件上传并发布到 NuGet 源。

1. 从[nuget 客户端工具网站](https://docs.microsoft.com/nuget/install-nuget-client-tools)下载 nuget.exe CLI 工具。
2. 运行 "nuget.exe pack [. nuspec 文件名]" 来创建 nupkg 文件。

### <a name="4-signing-your-extension-nuget-package"></a>4.为扩展 NuGet 包签名

需要使用来自受信任证书颁发机构（CA）的证书对扩展中包含的任何 .dll 文件进行签名。 默认情况下，当 Windows 管理中心在生产模式下运行时，将阻止未签名的 .dll 文件执行。

我们也强烈建议你对扩展 NuGet 包进行签名，以确保包的完整性，但这不是必需的步骤。

### <a name="5-test-your-extension-nuget-package"></a>5.测试扩展 NuGet 包

你的扩展包现已准备好进行测试！ 将 nupkg 文件上传到 NuGet 源，或将其复制到文件共享。 若要从其他源或文件共享查看和下载包，需要[将源配置更改](../configure/using-extensions.md#installing-extensions-from-a-different-feed)为指向 NuGet 源或文件共享。 测试时，请确保属性在 "扩展管理器" 中正确显示，并且可以成功安装和卸载扩展。

## <a name="publishing-your-extension-to-the-windows-admin-center-feed"></a>将扩展发布到 Windows 管理中心源

通过发布到 Windows 管理中心源，你可以使你的扩展可供任何 Windows 管理中心用户使用。 由于 Windows 管理中心 SDK 仍处于预览阶段，我们想要与你密切合作以帮助解决开发问题，并确保你能够向用户提供优质的产品和体验。

在发布扩展的初始版本之前，我们建议你在发布前至少2-3 周向 Microsoft 提交一个扩展审核请求，以确保有足够的时间进行查看，并根据需要对扩展进行任何更改。 一旦你的扩展准备好进行发布，你将需要将其发送给我们进行审阅，如果获得批准，我们会将其发布到源。

此后，如果要发布对扩展的更新，则需要提交另一个请求来查看。 根据更改的范围，更新评审的周转时间通常应较短。

### <a name="submit-an-extension-review-request-to-microsoft"></a>向 Microsoft 提交扩展评审请求

若要提交扩展审核请求，请输入以下信息，并以电子邮件的[wacextensionrequest@microsoft.com](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Review%20Request)形式发送到。 我们会在一周内回复你的电子邮件。

```
Windows Admin Center Extension Review Request
1. Name and email address of extension owner/developer (up to 3 users). If you will be releasing an extension on behalf of your company, provide your company email address.
2. Company name (Only required if you are releasing an extension on behalf of your company):
3. Extension name:
4. Release target date (estimate):
5. For new extension submission - Extension description (early design wire frames, screen mockups or product screenshots are highly recommended):
6. For extension update review – Description of changes (include product screenshots if UI has been significantly changed):
```

### <a name="submit-your-extension-package-for-review-and-publishing"></a>提交你的扩展包以供查看和发布

请确保按照上面的说明来[创建扩展包](#creating-an-extension-package)，并正确定义了 nuspec 文件并对文件进行签名。 我们还建议你有一个项目网站，其中包括以下内容：

- 扩展的详细说明，包括屏幕截图或视频
- 用于接收反馈或问题的电子邮件地址或网站功能

当你准备好发布扩展时，请向发送电子邮件[wacextensionrequest@microsoft.com](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Package%20Review) ，我们将提供有关如何向我们发送你的扩展包的说明。 收到你的包后，我们将查看并批准，并发布到 Windows 管理中心源。