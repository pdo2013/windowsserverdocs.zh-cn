---
title: 开发工具扩展
description: 开发工具扩展 Windows Admin Center SDK (项目 Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 092a97c1166f1090dd7c556f1ab86d42a1f46ee4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889268"
---
# <a name="develop-a-tool-extension"></a>开发工具扩展

>适用于：Windows Admin Center，Windows Admin Center 预览版

工具扩展是与 Windows Admin Center 来管理连接，如服务器或群集的用户交互的主要方法。 如果您单击 Windows Admin Center 主屏幕中的连接，连接将然后显示在左侧的导航窗格中的工具的列表。 当您单击工具上时，工具扩展可以加载并显示在右窗格中。

加载工具扩展时，它可以在目标服务器或群集上执行 WMI 调用或 PowerShell 脚本和在 UI 中显示的信息或执行基于用户输入的命令。 工具扩展定义应显示，从而导致一组不同的工具为每个解决方案的解决方案。

> [!NOTE]
> 不熟悉不同的扩展类型？ 详细了解如何[扩展性体系结构和扩展类型](understand-extensions.md)。

## <a name="prepare-your-environment"></a>准备你的环境

如果你尚未准备好，[准备您的环境](prepare-development-environment.md)通过安装依赖关系和所需的所有项目的全局系统必备组件。

## <a name="create-a-new-tool-extension-with-the-windows-admin-center-cli"></a>使用 Windows Admin Center CLI 创建新的工具扩展 ##

安装的所有依赖项后，你现可创建新的工具扩展。  创建或浏览到包含你的项目文件的文件夹，打开命令提示符，并将该文件夹设置为工作目录。  使用以前已安装 Windows Admin Center CLI，使用以下语法创建一个新的扩展：

``` cmd
wac create --company "{!Company Name}" --tool "{!Tool Name}"
```

| ReplTest1 | 说明 | 示例 |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | 你的公司名称 （包含空格） | ```Contoso Inc``` |
| ```{!Tool Name}``` | 你的工具名称 （包含空格） | ```Manage Foo Works``` |

下面是一个示例用法：

``` cmd
wac create --company "Contoso Inc" --tool "Manage Foo Works"
```

这将创建使用您所需的工具为指定的名称，将所有所需的模板文件复制到项目中，使用你的公司和工具名称配置文件的当前工作目录中的新文件夹。  

接下来，将目录更改为刚创建的文件夹，然后通过运行以下命令安装所需的本地依赖项：

``` cmd
npm install
```

完成此操作，你已设置所有需要在新的扩展加载到 Windows Admin Center。 

## <a name="add-content-to-your-extension"></a>将内容添加到您的扩展插件

现在，已使用 Windows Admin Center CLI 创建一个扩展，已准备好自定义内容。  有关你可以执行的操作的示例，请参阅以下指南：

- 添加[空模块](guides\add-module.md)
- 添加[iFrame](guides\add-iframe.md)
 
更多示例，请参阅我们[GitHub SDK 站点](https://aka.ms/wacsdk):
-  [开发人员工具](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/windows-admin-center-developer-tools)是一个完全正常运行的扩展，可以是旁加载到 Windows Admin Center，并包含一系列丰富的可浏览和使用自己的扩展中的示例功能和工具示例。

## <a name="customize-your-extensions-icon"></a>自定义扩展插件的图标

您可以自定义您的扩展工具列表中显示的图标。  若要执行此操作，修改所有```icon```中的条目```manifest.json```为扩展插件：

``` json
"icon": "{!icon-uri}",
```

| 值 | 说明 | 示例 uri |
| ----- | ----------- | ------- |
| ```{!icon-uri}``` | 图标资源的位置 | ```assets/foo-icon.svg``` |

注意：目前，自定义图标不可见时侧加载您在开发人员模式下的扩展。  作为一种解决方法，删除的内容```target```，如下所示：

``` json
"target": "",
```

此配置时才有效边载在开发人员模式下，所以务必要保留中包含的值```target```然后发布您的扩展插件之前对其进行还原。

## <a name="build-and-side-load-your-extension"></a>生成和端加载您的扩展插件

接下来，生成和端到 Windows Admin Center 加载你的扩展。  打开命令窗口，将目录更改为源目录，这样你就可以开始构建。

* 用 gulp 构建并提供服务：

    ``` cmd
    gulp build
    gulp serve -p 4201
    ```

请注意：你需要选择当前可用的端口。 请确保不要尝试使用正运行着 Windows Admin Center 的端口。

通过将本地服务项目附加到 Windows Admin Center，可使项目旁加载到 Windows Admin Center 的本地实例以进行测试。

* 在 Web 浏览器中启动 Windows Admin Center
* 打开调试程序 (F12)
* 打开控制台，并键入以下命令：

    ``` cmd
    MsftSme.sideLoad("http://localhost:4201")
    ```

*   刷新 Web 浏览器

项目现在将显示在工具列表中，名称旁边标有（旁加载）。

## <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>面向不同版本的 Windows Admin Center SDK

保持您的扩展插件的最新 SDK 更改和平台的更改非常简单。  阅读有关如何[面向不同版本](target-sdk-version.md)的 Windows Admin Center SDK。