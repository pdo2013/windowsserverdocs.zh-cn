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
ms.openlocfilehash: 08634e05d6b7450035324e8d925f2bb9df3b007e
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869577"
---
# <a name="prepare-your-development-environment"></a>准备开发环境

>适用于：Windows Admin Center、Windows Admin Center 预览版

让我们开始通过 Windows 管理中心 SDK 开发扩展！  在本文档中，我们将介绍如何启动并运行你的环境以生成和测试 Windows 管理中心的扩展。

> [!NOTE]
> 不熟悉 Windows Admin Center SDK？  了解有关 [Windows Admin Center 的扩展](extensibility-overview.md) 的详细信息

要准备开发环境，请执行以下步骤：

## <a name="install-prerequisites"></a>安装必备组件

要开始使用 SDK 进行开发，请下载并安装以下必备组件：

* [Windows 管理中心](https://aka.ms/WACDownloadPage)（GA 或预览版）
* Visual Studio 或 [Visual Studio Code](http://code.visualstudio.com)
* [节点包管理器](https://npmjs.com/get-npm)（8.12.0 或更高版本）
* [NuGet](https://www.nuget.org/downloads)（用于发布扩展）

> [!NOTE]
> 你需要在开发人员模式下安装并运行 Windows Admin Center，以执行以下步骤。 开发人员模式允许 Windows Admin Center 加载未签名的扩展包。
>
>  要启用开发人员模式，请从命令行使用参数 DEV_MODE = 1 安装 Windows Admin Center。 在以下示例中，将 ```<version>``` 替换为正在安装的版本，即 ```WindowsAdminCenter1809.msi```。
>
> ```msiexec /i WindowsAdminCenter<version>.msi DEV_MODE=1```

## <a name="install-global-dependencies"></a>安装全局依赖项

接下来，通过 Node 包管理器安装或更新项目所需的依赖项。 这些依赖项将全局安装，并适用于所有项目。

```
npm install -g npm

npm install -g @angular/cli@1.6.5

npm install -g gulp
npm install -g typescript
npm install -g tslint
npm install -g windows-admin-center-cli
```

>[!NOTE]
>你可以安装的@angular/cli更高版本，但请注意，如果你安装的版本高于1.6.5，则在 gulp 生成步骤中将收到一条警告，指示本地 cli 版本与安装的版本不匹配。

## <a name="next-steps"></a>后续步骤

准备好环境后，就可以开始创建内容了。

- 创建[工具](develop-tool.md)扩展
- 创建[解决方案](develop-solution.md)扩展
- 创建[网关插件](develop-gateway-plugin.md)
- 通过我们的[指南](guides.md)了解详细信息

## <a name="sdk-design-toolkit"></a>SDK 设计工具包

查看 Windows 管理中心[SDK 设计工具包](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip)！ 此工具包旨在帮助您使用 Windows 管理中心样式、控件和页面模板快速模拟 PowerPoint 中的扩展。 开始编码之前，请查看你的扩展在 Windows 管理中心中的外观。

