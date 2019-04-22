---
title: 发布 Windows Admin Center 的扩展
description: 发布 Windows Admin Center (项目 Honolulu) 的扩展
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 762bd4613fa8ad6cdfb5b44745a7ce78b331499d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820418"
---
# <a name="publishing-extensions"></a>发布扩展

>适用于：Windows Admin Center，Windows Admin Center 预览版

后您开发您的扩展插件，你将想要将其发布，使其可供其他人来测试或使用。 具体取决于你的受众和发布的目的，有几个选项，下面我们将介绍的步骤以及用于发布的要求。

## <a name="publishing-options"></a>发布选项

有用于 Windows Admin Center 支持的可配置的包源的三个主要选项：
* Microsoft 的公共 Windows Admin Center NuGet 源
* 私有 NuGet 源
* 本地或网络文件共享

### <a name="publishing-to-the-windows-admin-center-extension-feed"></a>发布到源的 Windows Admin Center 扩展

默认情况下，Windows Admin Center 连接到 NuGet 源在 Microsoft Windows Admin Center 产品团队维护的。 已发布到此数据源并可供 Windows Admin Center 用户，可能是由 Microsoft 开发的新扩展的早期预览版本。 计划生成和公开发布扩展的外部开发人员可能还[提交请求](#publishing-your-extension-to-the-windows-admin-center-feed)发布到此数据源。

### <a name="publishing-to-a-different-nuget-feed"></a>发布到不同的 NuGet 源

您也可以创建自己的 NuGet 源，以便将你的扩展发布到使用多个[设置的私有源或使用 NuGet 承载服务的不同选项](https://docs.microsoft.com/nuget/hosting-packages/overview)。 NuGet 源必须支持 NuGet v2 API。 由于 Windows Admin Center 当前不支持源身份验证，源必须配置为允许读取访问权限的任何人。

### <a name="publishing-to-a-file-share"></a>发布到文件共享

若要限制访问你的扩展到你的组织或一组有限的人，可以作为源的扩展使用 SMB 文件共享。 在这种情况下，将允许访问到数据源的应用的文件共享和文件夹权限。

## <a name="preparing-your-extension-for-release"></a>准备你的扩展版本

请确保您阅读并考虑以下开发主题：

- [控制工具的可见性](guides/dynamic-tool-display.md)
- [字符串和本地化](guides/strings-localization.md)

### <a name="consider-releasing-as-a-preview-release"></a>请考虑作为预览版发布

如果发布预览版本的扩展以供评估目的，我们建议您：

- .Nuspec 文件中的扩展的标题末尾追加"（预览）"
- 解释.nuspec 文件中的扩展的说明中的限制

## <a name="creating-an-extension-package"></a>创建一个扩展包

Windows Admin Center 可以利用 NuGet 包和用于分发和下载扩展的源。  为了使要发运的货包，需要生成包含你的插件和扩展的 NuGet 包。  一个包可以包含 UI 扩展以及网关插件和下一节将引导您完成过程。

### <a name="1-build-your-extension"></a>1.构建您的扩展插件

一旦你已准备好开始打包你的扩展，在文件系统上创建一个新目录，打开一个控制台并 CD 到它。  这将是我们将使用包含所有 nuspec 和内容目录构成我们的程序包的根目录。  本文档的持续时间内，我们将引用为"NuGet 包"此文件夹。

#### <a name="ui-extensions"></a>UI 扩展

若要开始收集 UI 扩展所需的所有内容过程，请运行所需的工具上的"gulp 生成"，并确保生成已成功。  此进程包一起在一个名为"捆绑"文件夹中的所有组件都位于您的扩展 （在相同级别的 src 目录中） 的根目录中。  将此目录及其所有复制的"NuGet 包"文件夹的内容。

#### <a name="gateway-plugins"></a>网关插件

使用生成基础结构 （这可能是打开 Visual Studio 并单击生成按钮一样简单），编译和生成你的插件。  生成输出目录中，打开和复制 dll 表示你的插件，并将其放在"NuGet 包"目录中名为"包"的新文件夹中。  不需要复制 FeatureInterface dll，只是表示你的代码的 dll。

### <a name="2-create-the-nuspec-file"></a>2.创建.nuspec 文件

若要创建 NuGet 包，您需要首先创建.nuspec 文件。 .Nuspec 文件是一个包含 NuGet 包元数据的 XML 清单。 此清单可同时用于生成包并向使用者提供的信息。  将此文件放置在"NuGet 包"文件夹的根目录。

下面是一个示例.nuspec 文件和必需或推荐的属性的列表。 有关完整的架构，请参阅[.nuspec 引用](https://docs.microsoft.com/nuget/reference/nuspec)。 .Nuspec 文件保存到与所选的文件名称的项目的根文件夹。

> [!IMPORTANT]
> ```<id>``` .Nuspec 文件中的值必须与匹配```"name"```在项目中的值```manifest.json```文件，或者你已发布的扩展不会加载已成功在 Windows Admin Center 中。

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

| 属性名 | 建议使用 / 所需 | 描述 |
| ---- | ---- | ---- |
| packageType | 必需 | 使用"WindowsAdminCenterExtension"这是为 Windows Admin Center 扩展定义的 NuGet 包类型。 |
| id | 必需 | 在源内的唯一包标识符。 此值必须以匹配你的项目的 manifest.json 文件中的"名称"值。  请参阅[选择唯一包标识符](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number)有关的指南。 |
| title | 所需的发布到 Windows Admin Center 源 | 包显示在 Windows Admin Center 扩展管理器的友好名称。 |
| version | 必需 | 扩展版本。 使用[语义化版本控制 （SemVer 约定）](http://semver.org/spec/v1.0.0.html)建议但不是必需的。 |
| 作者 | 必需 | 如果发布你的公司代表，请使用你的公司名称。 |
| description | 必需 | 提供扩展的功能的说明。 |
| iconUrl | 发布到 Windows Admin Center 源时，建议使用 | 若要显示在扩展管理器图标的 URL。 |
| projectUrl | 所需的发布到 Windows Admin Center 源 | 扩展的网站 URL。 如果没有另一个网站，NuGet 源上的包网页中使用的 URL。 |
| licenseUrl | 所需的发布到 Windows Admin Center 源 | 扩展的最终用户许可协议的 URL。 |
| 文件 | 必需 | 这两个设置设置 Windows Admin Center 所预期的用户界面扩展和网关插件文件夹结构。 |

### <a name="3-build-the-extension-nuget-package"></a>3.生成扩展 NuGet 包

使用上面创建的.nuspec 文件，现在将创建 NuGet 包.nupkg 文件上传和发布到 NuGet 源。

1. 下载的 nuget.exe CLI 工具[NuGet 客户端工具网站](https://docs.microsoft.com/nuget/install-nuget-client-tools)。
2. 运行"nuget.exe 包 [.nuspec 文件名称]"以创建.nupkg 文件。

### <a name="4-signing-your-extension-nuget-package"></a>4.在扩展 NuGet 程序包进行签名

在扩展中包括的任何.dll 文件都需要使用从受信任证书颁发机构 (CA) 证书进行签名。 默认情况下，将从在生产模式中运行 Windows Admin Center 时正在执行阻止未签名的.dll 文件。

我们还强烈建议你注册扩展 NuGet 包，以确保包完整性，但这不是所必需的步骤。

### <a name="5-test-your-extension-nuget-package"></a>5.测试扩展 NuGet 包

扩展包现已准备好进行测试 ！ 将.nupkg 文件上传到 NuGet 源，或将其复制到文件共享。 若要查看并从不同的源或文件共享下载包，你将需要[更改源的配置](../configure/using-extensions.md#installing-extensions-from-a-different-feed)指向 NuGet 源或文件共享。 在测试时，请确保属性正确显示在扩展管理器中，并且可以成功地安装和卸载你的扩展。

## <a name="publishing-your-extension-to-the-windows-admin-center-feed"></a>将你的扩展发布到 Windows Admin Center 源

通过发布到 Windows Admin Center 源，您可以让您的扩展提供的任何 Windows Admin Center 用户。 由于 Windows Admin Center SDK 仍处于预览状态，我们想要密切合作，与你来帮助解决开发问题，请确保您便能够交付高质量的产品和体验对你的用户。

在发布您的扩展插件的初始版本之前，我们建议您在至少 2-3 周之前 release，以确保我们具有足够的时间来查看和，以便对您的扩展插件进行任何更改，如有必要提交给 Microsoft 的扩展评审请求。 准备好要发布您的扩展插件后，将需要将其发送给我们进行审阅，而如果获得批准，我们会将其发布到你的数据源。

之后，如果你想要发布您的扩展的更新，您需要提交进行审阅的另一个请求。 根据更改的范围，更新评审周转时间通常应该更短的时间。

### <a name="submit-an-extension-review-request-to-microsoft"></a>提交给 Microsoft 的扩展评审请求

若要提交扩展评审请求，请输入以下信息并发送作为电子邮件至[ wacextensionrequest@microsoft.com ](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Review%20Request)。 我们将一周内回复你的电子邮件。

```
Windows Admin Center Extension Review Request
1. Name and email address of extension owner/developer (up to 3 users). If you will be releasing an extension on behalf of your company, provide your company email address.
2. Company name (Only required if you are releasing an extension on behalf of your company):
3. Extension name:
4. Release target date (estimate):
5. For new extension submission - Extension description (early design wire frames, screen mockups or product screenshots are highly recommended):
6. For extension update review – Description of changes (include product screenshots if UI has been significantly changed):
```

### <a name="submit-your-extension-package-for-review-and-publishing"></a>提交以进行评审和发布扩展包

请确保按照上述说明[创建一个扩展包](#creating-an-extension-package)并正确定义.nuspec 文件和文件进行签名。 我们还建议你拥有其中包括的项目网站：

- 您的扩展包括屏幕快照或视频的详细的说明
- 电子邮件地址或网站功能，以接收反馈或问题

当准备好要发布扩展时，发送电子邮件至[ wacextensionrequest@microsoft.com ](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Package%20Review)和我们将提供有关如何向我们发送您的扩展包的说明。 一旦收到您的包，我们将查看并批准的如果将发布到 Windows Admin Center 源。