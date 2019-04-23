---
title: 准备开发环境
description: 准备开发环境 Windows Admin Center SDK (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.date: 09/18/2018
ms.prod: windows-server-threshold
ms.openlocfilehash: 7b1a0672ee374f3e2d1339c43576db0e5cabdc36
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834758"
---
# <a name="prepare-your-development-environment"></a>准备开发环境

>适用于：Windows Admin Center，Windows Admin Center 预览版

让我们开始使用 Windows Admin Center SDK 开发扩展！  在此文档中，我们将介绍您的环境启动并运行用于生成和测试 Windows Admin Center 的扩展的过程。

> [!NOTE]
> 不熟悉 Windows Admin Center SDK？  了解有关 [Windows Admin Center 的扩展](extensibility-overview.md) 的详细信息

要准备开发环境，请执行以下步骤：

## <a name="install-prerequisites"></a>安装必备组件

要开始使用 SDK 进行开发，请下载并安装以下必备组件：

* [Windows Admin Center](https://aka.ms/WACDownloadPage) （GA 或预览版版本）
* Visual Studio 或 [Visual Studio Code](http://code.visualstudio.com)
* [节点包管理器](https://npmjs.com/get-npm)(8.12.0 或更高版本)
* [NuGet](https://www.nuget.org/downloads)（用于发布扩展）

> [!NOTE]
> 你需要在开发人员模式下安装并运行 Windows Admin Center，以执行以下步骤。 开发人员模式允许 Windows Admin Center 加载未签名的扩展包。
>
>  要启用开发人员模式，请从命令行使用参数 DEV_MODE = 1 安装 Windows Admin Center。 在以下示例中，将 ```<version>``` 替换为正在安装的版本，即 ```WindowsAdminCenter1809.msi```。
>
> ```msiexec /i WindowsAdminCenter<version>.msi DEV_MODE=1```

## <a name="install-global-dependencies"></a>安装全局依赖项

接下来，安装或更新你的项目，使用节点包管理器所需的依赖项。 这些依赖项将全局安装，并适用于所有项目。

```
npm install -g npm

npm install -g @angular/cli@1.6.5

npm install -g gulp
npm install -g typescript
npm install -g tslint
npm install -g windows-admin-center-cli
```

>[!NOTE]
>可以安装更高版本的@angular/cli，但是请注意，如果安装的版本大于 1.6.5，你会收到警告 gulp 生成步骤本地 cli 版本与已安装的版本不匹配。

## <a name="next-steps"></a>后续步骤

现在，对环境进行准备，您就可以开始创建内容。

- 创建[工具](develop-tool.md)扩展
- 创建[解决方案](develop-solution.md)扩展
- 创建[网关插件](develop-gateway-plugin.md)
- 通过我们的[指南](guides.md)了解详细信息

## <a name="sdk-design-toolkit"></a>SDK 设计工具包

签出我们 Windows Admin Center [SDK 设计工具包](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip)！ 此工具包旨在帮助您快速模拟在 PowerPoint 中使用 Windows Admin Center 样式、 控件和页面模板的扩展。 请参阅什么扩展看起来像是在 Windows Admin Center 中开始编码之前 ！

