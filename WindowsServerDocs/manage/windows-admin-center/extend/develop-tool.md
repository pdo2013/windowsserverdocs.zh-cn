---
title: 开发工具扩展
description: 开发工具扩展 Windows 管理中心 SDK （Project Honolulu）
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: de2cbf3a47771555eef02cd7d18f93b2b33227b3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406908"
---
# <a name="develop-a-tool-extension"></a>开发工具扩展

>适用于：Windows Admin Center、Windows Admin Center 预览版

工具扩展是用户与 Windows 管理中心交互以管理连接（如服务器或群集）的主要方式。 当你在 Windows 管理中心主页屏幕上单击连接并连接时，会在左侧导航窗格中显示工具列表。 单击工具时，将加载该工具扩展并将其显示在右窗格中。

加载某个工具扩展后，它可以在目标服务器或群集上执行 WMI 调用或 PowerShell 脚本，并在 UI 中显示信息或根据用户输入执行命令。 工具扩展定义应该为其显示的解决方案，从而为每个解决方案提供一组不同的工具。

> [!NOTE]
> 不熟悉不同的扩展类型？ 了解有关[扩展性体系结构和扩展类型](understand-extensions.md)的详细信息。

## <a name="prepare-your-environment"></a>准备你的环境

如果尚未准备好，请通过安装所有项目所需的依赖项和全局必备组件来[准备环境](prepare-development-environment.md)。

## <a name="create-a-new-tool-extension-with-the-windows-admin-center-cli"></a>使用 Windows 管理中心 CLI 创建新的工具扩展 ##

所有依赖项安装完成后，即可创建新的工具扩展。  创建或浏览到包含项目文件的文件夹，打开命令提示符，然后将该文件夹设置为工作目录。  使用之前安装的 Windows 管理中心 CLI 创建新扩展，并使用以下语法：

``` cmd
wac create --company "{!Company Name}" --tool "{!Tool Name}"
```

| ReplTest1 | 说明 | 示例 |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | 公司名称（包含空格） | ```Contoso Inc``` |
| ```{!Tool Name}``` | 工具名称（包含空格） | ```Manage Foo Works``` |

下面是一个示例用法：

``` cmd
wac create --company "Contoso Inc" --tool "Manage Foo Works"
```

这将使用为工具指定的名称在当前工作目录中创建一个新文件夹，将所有必需的模板文件复制到你的项目中，并使用你的公司和工具名称配置文件。  

接下来，将目录更改为刚创建的文件夹，然后通过运行以下命令安装所需的本地依赖项：

``` cmd
npm install
```

完成此操作后，已设置将新扩展加载到 Windows 管理中心所需的一切。 

## <a name="add-content-to-your-extension"></a>向扩展添加内容

现在，已使用 Windows 管理中心 CLI 创建了扩展，接下来可以自定义内容。  有关可执行的操作的示例，请参阅以下指南：

- 添加[空模块](guides/add-module.md)
- 添加[iFrame](guides/add-iframe.md)
 
有关更多示例，请参阅[GITHUB SDK 网站](https://aka.ms/wacsdk)：
-  [开发人员工具](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/windows-admin-center-developer-tools)是一种完全正常运行的扩展插件，可以将其放入 Windows 管理中心，其中包含一系列丰富的示例功能和工具示例，你可以在自己的扩展中浏览和使用它们。

## <a name="customize-your-extensions-icon"></a>自定义扩展的图标

你可以在工具列表中自定义为你的扩展显示的图标。  为此，请在 @no__t 中修改扩展的所有 ```manifest.json``` 项：

``` json
"icon": "{!icon-uri}",
```

| ReplTest1 | 说明 | 示例 uri |
| ----- | ----------- | ------- |
| ```{!icon-uri}``` | 图标资源的位置 | ```assets/foo-icon.svg``` |

注意：目前，在开发模式下加载扩展时，不会显示自定义图标。  一种解决方法是删除 @no__t 的内容，如下所示：

``` json
"target": "",
```

此配置仅适用于在开发模式下进行的端加载，因此，请务必保留 ```target``` 中包含的值，然后在发布扩展之前还原它。

## <a name="build-and-side-load-your-extension"></a>生成和加载扩展

接下来，构建扩展并将其加载到 Windows 管理中心。  打开一个命令窗口，将目录更改为你的源目录，然后你就可以生成了。

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

## <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>面向不同版本的 Windows 管理中心 SDK

通过 SDK 更改和平台更改，使扩展保持最新状态非常简单。  阅读有关如何以[不同版本](target-sdk-version.md)的 WINDOWS 管理中心 SDK 为目标的信息。