---
title: 开发解决方案扩展
description: 开发解决方案扩展 Windows 管理中心 SDK （Project Honolulu）
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 6ac9c6296fdf9159c9f50a1304dd345932052ac9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357153"
---
# <a name="develop-a-solution-extension"></a>开发解决方案扩展

>适用于：Windows Admin Center、Windows Admin Center 预览版

解决方案主要定义要通过 Windows 管理中心进行管理的唯一对象类型。  默认情况下，Windows 管理中心附带以下解决方案/连接类型：

* Windows Server 连接
* Windows 电脑连接
* 故障转移群集连接
* 超聚合群集连接

从 "Windows 管理中心连接" 页中选择连接时，将加载该连接类型的解决方案扩展，并且 Windows 管理中心将尝试连接到目标节点。 如果连接成功，则会加载解决方案扩展的 UI，并且 Windows 管理中心会在左侧导航窗格中显示该解决方案的工具。

如果要为上述默认连接类型定义的服务（例如网络交换机）或其他无法通过计算机名称发现的硬件生成管理 GUI，你可能需要创建自己的解决方案扩展。

> [!NOTE]
> 不熟悉不同的扩展类型？ 了解有关[扩展性体系结构和扩展类型](understand-extensions.md)的详细信息。

## <a name="prepare-your-environment"></a>准备你的环境

如果尚未准备好，请通过安装所有项目所需的依赖项和全局必备组件来[准备环境](prepare-development-environment.md)。

## <a name="create-a-new-solution-extension-with-the-windows-admin-center-cli"></a>使用 Windows 管理中心 CLI 创建新的解决方案扩展 ##

所有依赖项安装完成后，即可创建新的解决方案扩展。  创建或浏览到包含项目文件的文件夹，打开命令提示符，然后将该文件夹设置为工作目录。  使用之前安装的 Windows 管理中心 CLI 创建新扩展，并使用以下语法：

```
wac create --company "{!Company Name}" --solution "{!Solution Name}" --tool "{!Tool Name}"
```

| ReplTest1 | 说明 | 示例 |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | 公司名称（包含空格） | ```Contoso Inc``` |
| ```{!Solution Name}``` | 解决方案名称（包含空格） | ```Contoso Foo Works Suite``` |
| ```{!Tool Name}``` | 工具名称（包含空格） | ```Manage Foo Works``` |

下面是一个示例用法：

```
wac create --company "Contoso Inc" --solution "Contoso Foo Works Suite" --tool "Manage Foo Works"
```

这将使用您为解决方案指定的名称在当前工作目录中创建一个新文件夹，将所有必需的模板文件复制到您的项目中，并使用您的公司、解决方案和工具名称配置文件。  

接下来，将目录更改为刚创建的文件夹，然后通过运行以下命令安装所需的本地依赖项：

```
npm install
```

完成此操作后，已设置将新扩展加载到 Windows 管理中心所需的一切。 

## <a name="add-content-to-your-extension"></a>向扩展添加内容

现在，已使用 Windows 管理中心 CLI 创建了扩展，接下来可以自定义内容。  有关可执行的操作的示例，请参阅以下指南：

- 添加[空模块](guides/add-module.md)
- 添加[iFrame](guides/add-iframe.md)
- 创建[自定义连接提供程序](guides/create-connection-provider.md)
- 修改[根导航行为](guides/modify-root-navigation.md)
 
有关更多示例，请参阅[GITHUB SDK 网站](https://aka.ms/wacsdk)：
-  [开发人员工具](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/windows-admin-center-developer-tools)是一种完全正常运行的扩展插件，可以将其放入 Windows 管理中心，其中包含一系列丰富的示例功能和工具示例，你可以在自己的扩展中浏览和使用它们。

## <a name="build-and-side-load-your-extension"></a>生成和加载扩展

接下来，构建扩展并将其加载到 Windows 管理中心。  打开一个命令窗口，将目录更改为你的源目录，然后你就可以生成了。

* 用 gulp 构建并提供服务：

    ```
    gulp build
    gulp serve -p 4201
    ```

请注意：你需要选择当前可用的端口。 请确保不要尝试使用正运行着 Windows Admin Center 的端口。

通过将本地服务项目附加到 Windows Admin Center，可使项目旁加载到 Windows Admin Center 的本地实例以进行测试。

* 在 Web 浏览器中启动 Windows Admin Center
* 打开调试程序 (F12)
* 打开控制台，并键入以下命令：

    ```
    MsftSme.sideLoad("http://localhost:4201")
    ```

*   刷新 Web 浏览器

项目现在将显示在工具列表中，名称旁边标有（旁加载）。

## <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>面向不同版本的 Windows 管理中心 SDK

通过 SDK 更改和平台更改，使扩展保持最新状态非常简单。  阅读有关如何以[不同版本](target-sdk-version.md)的 WINDOWS 管理中心 SDK 为目标的信息。