---
title: 发布 Windows Admin Center 的扩展
description: 发布扩展 Windows Admin center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 762bd4613fa8ad6cdfb5b44745a7ce78b331499d
ms.sourcegitcommit: 3883eebbba70bfea0221e510863ee1a724a5f926
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2018
ms.locfileid: "5783749"
---
# 发布扩展

>适用于：Windows Admin Center、Windows Admin Center 预览版

一旦你之前开发过你的扩展，你会想要发布它，并将其提供给其他人能够在测试或使用。 根据你的受众和发布的目的，有几种选项，我们将介绍下面的步骤和发布要求以及它。

## 发布选项

有三个主要用于 Windows Admin Center 支持的可配置的程序包源选项：
* Microsoft 的公共 Windows Admin Center NuGet 源
* 专用的 NuGet 订阅源
* 本地或网络文件共享

### 发布到 Windows Admin Center 扩展源

默认情况下，Windows Admin Center 连接到 NuGet 维护 Microsoft Windows Admin Center 产品团队的源。 早期的预览版本的 Microsoft 开发的新扩展可能会发布到此订阅源，并提供给 Windows Admin Center 的用户。 规划生成和公开发布扩展的外部开发人员还可以[提交请求](#publishing-your-extension-to-the-windows-admin-center-feed)将发布到此订阅源。

### 发布到不同的 NuGet 源

你还可以创建自己源将发布到使用一个很多[不同的设置专用的源或使用 NuGet 选项托管服务](https://docs.microsoft.com/nuget/hosting-packages/overview)对扩展的 NuGet。 NuGet 源必须支持 NuGet v2 API。 Windows Admin Center 当前不支持订阅源的身份验证，因为源需要配置为允许的读取访问权限的任何人。

### 发布到的文件共享

若要限制对你的组织或一组有限的人的扩展的访问，你可以为源扩展使用 SMB 文件共享。 在此情况下，将以允许对源的访问应用的文件共享和文件夹的权限。

## 准备发布扩展

请确保阅读，考虑以下开发主题：

- [控制工具的可见性](guides/dynamic-tool-display.md)
- [字符串和本地化](guides/strings-localization.md)

### 请考虑发布作为预览版本

如果你要发布供试用你扩展的预览版本，我们建议你：

- 将"（预览版）"附加到你的扩展.nuspec 文件中的主题作品的末尾
- 介绍.nuspec 文件中的扩展的描述中的限制

## 创建扩展包

利用 Windows Admin Center 的 NuGet 程序包和分发和下载扩展的源。  为了让你的程序包交付，你将需要生成包含你的插件和扩展的 NuGet 包。  单个程序包可以包含 UI 扩展以及网关插件，并且以下部分将指导你完成此过程。

### 1.生成你的扩展

只要你准备开始菜单打包你的扩展，文件系统上创建一个新的目录，打开控制台，并 CD 到其中。  这将是我们将包含所有 nuspec 和内容目录，它会使我们程序包的根目录。  本文档的持续时间内，我们将引用"NuGet 程序包"为此文件夹。

#### UI 扩展

若要开始上收集所需的 UI 扩展名的所有内容的进程，运行取决于你的工具的"gulp 构建"，并确保生成操作成功。  此过程程序包，一起在一个名为"bundle"文件夹中的所有组件都位于你的扩展 （在相同级别的源目录） 的根目录中。  将此目录及其所有复制的"NuGet 程序包"文件夹的内容。

#### 网关插件

使用你生成的基础结构 （这可能是只要打开 Visual Studio，然后单击生成按钮），编译并生成你的插件。  打开你生成的输出目录，并复制表示你的插件，将 dll 并将它们放在名为"程序包"的"NuGet 程序包"目录中的新文件夹。  你不需要复制 FeatureInterface dll，只需将 dll 表示你的代码。

### 2.创建.nuspec 文件

若要创建的 NuGet 程序包，你需要首先创建一个.nuspec 文件。 .Nuspec 文件是包含 NuGet 包元数据的 XML 清单。 此清单用于生成程序包并向客户提供的信息。  将该文件放置在"NuGet 程序包"文件夹的根目录。

下面是示例.nuspec 文件和需要或推荐属性的列表。 有关完整的架构，请参阅[.nuspec 引用](https://docs.microsoft.com/nuget/reference/nuspec)。 .Nuspec 文件保存到你选择的文件名与你的项目根文件夹。

> [!IMPORTANT]
> ```<id>``` .Nuspec 文件中的值应匹配```"name"```在项目中的值```manifest.json```文件，否则你已发布的扩展不会成功加载在 Windows Admin Center 中。

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

#### 需要或推荐属性

| 属性名称 | 必需 / 建议 | 说明 |
| ---- | ---- | ---- |
| ： 键入包 | 必需 | 使用"WindowsAdminCenterExtension"这是为 Windows Admin Center 扩展定义的 NuGet 程序包类型。 |
| id | 必需 | 馈送中的唯一程序包标识符。 此值必须匹配你的项目 manifest.json 文件中的"名称"值。  获取指导信息，请参阅[选择一个唯一的程序包标识符](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number)。 |
| title | 所需的发布到 Windows Admin Center 源 | 将显示在 Windows Admin Center 扩展管理器的包的友好名称。 |
| version | 必需 | 扩展版本。 使用[语义进行版本控制 （SemVer 约定）](http://semver.org/spec/v1.0.0.html)是推荐但不是必需的。 |
| 作者 | 必需 | 如果发布代表你的公司，使用你的公司的名称。 |
| description | 必需 | 提供的扩展的功能的说明。 |
| iconUrl | 建议在发布到 Windows Admin Center 源 | 若要在扩展管理器中显示的图标 URL。 |
| projectUrl | 所需的发布到 Windows Admin Center 源 | 扩展的网站的 URL。 如果你没有单独的网站，用于在源 NuGet 程序包网页的 URL。 |
| licenseUrl | 所需的发布到 Windows Admin Center 源 | 扩展的最终用户许可协议的 URL。 |
| 文件 | 必需 | 这两个设置设置 Windows Admin Center 需要的 UI 扩展和网关插件的文件夹结构。 |

### 3.生成扩展 NuGet 程序包

使用上面创建的.nuspec 文件，你现在可以创建你可以上传并发布到源 NuGet 的 NuGet 包.nupkg 文件。

1. 从[NuGet 客户端工具网站](https://docs.microsoft.com/nuget/install-nuget-client-tools)下载 nuget.exe CLI 工具。
2. 运行"nuget.exe 包 [.nuspec 文件名称]"以创建.nupkg 文件。

### 4.签名扩展 NuGet 程序包

你的扩展中包含任何.dll 文件时需要使用从受信任证书颁发机构 (CA) 证书进行签名。 默认情况下，未签名的.dll 文件将被阻止的生产模式下运行 Windows Admin Center 时执行。

我们还强烈建议你先签署扩展 NuGet 程序包以确保该程序包的完整性，但这不是必需的步骤。

### 5.测试扩展 NuGet 程序包

扩展包现已准备好进行测试 ！ .Nupkg 文件上载到 NuGet 源或将其复制到的文件共享。 若要查看和从不同的源或文件共享下载程序包，你将需要[更改你的订阅源的配置](../configure/using-extensions.md#installing-extensions-from-a-different-feed)为指向你的 NuGet 源或文件共享。 测试时，请确保属性正确显示在扩展管理器并在成功安装和卸载扩展。

## 发布到 Windows Admin Center 的扩展源

通过发布到 Windows Admin Center 订阅源，你可以使你的扩展可向任何 Windows Admin Center 用户。 Windows Admin Center SDK 仍处于预览，因为我们想要与你以帮助解决开发问题，请确保你能够提供高质量产品并给你的用户体验密切合作。

释放你的扩展的初始版本，我们建议你在之前版本以确保我们有足够的时间来查看和可以对你的扩展中进行任何更改，如有必要至少需要 2-3 周提交到 Microsoft 的扩展评价请求。 准备好发布你的扩展后，你将需要将其发送给我们回顾，并且如果批准，我们会将其发布到你的源。

之后，如果你想要发布到扩展更新，你将需要提交对评价的其他请求。 尽管具体取决于更改的范围，更新评论的周转时间通常应短。

### 提交到 Microsoft 的扩展评价请求

若要提交扩展评价请求，输入以下信息，并作为到[wacextensionrequest@microsoft.com](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Review%20Request)一封电子邮件发送。 我们将在一周内回复你的电子邮件。

```
Windows Admin Center Extension Review Request
1. Name and email address of extension owner/developer (up to 3 users). If you will be releasing an extension on behalf of your company, provide your company email address.
2. Company name (Only required if you are releasing an extension on behalf of your company):
3. Extension name:
4. Release target date (estimate):
5. For new extension submission - Extension description (early design wire frames, screen mockups or product screenshots are highly recommended):
6. For extension update review – Description of changes (include product screenshots if UI has been significantly changed):
```

### 提交审阅和发布你扩展的程序包

请确保按照上述说明[创建扩展包](#creating-an-extension-package)和.nuspec 文件的定义是正确的文件进行签名。 我们还建议你拥有某个项目网站，包括：

- 扩展包括屏幕截图或视频的详细的说明
- 电子邮件地址或网站功能，以接收的反馈或问题

当你准备好发布你的扩展时，将一封电子邮件发送到[wacextensionrequest@microsoft.com](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Package%20Review)以及我们将提供有关如何向我们发送你的扩展包的说明。 一旦我们收到你的程序包，我们将查看并批准，如果将发布到 Windows Admin Center 源。