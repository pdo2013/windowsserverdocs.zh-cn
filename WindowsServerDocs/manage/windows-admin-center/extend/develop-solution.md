---
title: 开发解决方案扩展
description: 开发解决方案扩展 Windows Admin Center SDK (项目 Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 268a7d2833f73e9fab006501e9b3dc261d1b1d9e
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452572"
---
# <a name="develop-a-solution-extension"></a>开发解决方案扩展

>适用于：Windows Admin Center，Windows Admin Center 预览版

解决方案主要定义要通过 Windows Admin Center 管理的对象的唯一类型。  这些解决方案/连接类型是附带默认情况下 Windows Admin Center:

* Windows Server 的连接
* Windows PC 的连接
* 故障转移群集连接
* 超聚合群集连接

当你从 Windows Admin Center 连接页中选择连接时，加载该连接的类型的解决方案扩展插件，和 Windows Admin Center 将尝试连接到目标节点。 如果连接成功，该解决方案将会加载扩展的 UI，以及 Windows Admin Center 会在左侧的导航窗格中显示该解决方案的工具。

如果你想要生成不由上面的默认连接类型，此类网络交换机或发现其他硬件由计算机名称的服务的管理 GUI，您可能想要创建自己的解决方案的扩展。

> [!NOTE]
> 不熟悉不同的扩展类型？ 详细了解如何[扩展性体系结构和扩展类型](understand-extensions.md)。

## <a name="prepare-your-environment"></a>准备你的环境

如果你尚未准备好，[准备您的环境](prepare-development-environment.md)通过安装依赖关系和所需的所有项目的全局系统必备组件。

## <a name="create-a-new-solution-extension-with-the-windows-admin-center-cli"></a>使用 Windows Admin Center CLI 中创建一个新的解决方案扩展 ##

安装的所有依赖项后，你现可创建新的解决方案扩展名。  创建或浏览到包含你的项目文件的文件夹，打开命令提示符，并将该文件夹设置为工作目录。  使用以前已安装 Windows Admin Center CLI，使用以下语法创建一个新的扩展：

```
wac create --company "{!Company Name}" --solution "{!Solution Name}" --tool "{!Tool Name}"
```

| ReplTest1 | 说明 | 示例 |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | 你的公司名称 （包含空格） | ```Contoso Inc``` |
| ```{!Solution Name}``` | 你的解决方案名称 （包含空格） | ```Contoso Foo Works Suite``` |
| ```{!Tool Name}``` | 你的工具名称 （包含空格） | ```Manage Foo Works``` |

下面是一个示例用法：

```
wac create --company "Contoso Inc" --solution "Contoso Foo Works Suite" --tool "Manage Foo Works"
```

这将创建使用您为解决方案指定的名称，将所有所需的模板文件复制到项目中，使用你的公司、 解决方案和工具名称配置文件的当前工作目录中的新文件夹。  

接下来，将目录更改为刚创建的文件夹，然后通过运行以下命令安装所需的本地依赖项：

```
npm install
```

完成此操作，你已设置所有需要在新的扩展加载到 Windows Admin Center。 

## <a name="add-content-to-your-extension"></a>将内容添加到您的扩展插件

现在，已使用 Windows Admin Center CLI 创建一个扩展，已准备好自定义内容。  有关你可以执行的操作的示例，请参阅以下指南：

- 添加[空模块](guides/add-module.md)
- 添加[iFrame](guides/add-iframe.md)
- 创建[自定义连接提供程序](guides/create-connection-provider.md)
- 修改[根导航行为](guides/modify-root-navigation.md)
 
更多示例，请参阅我们[GitHub SDK 站点](https://aka.ms/wacsdk):
-  [开发人员工具](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/windows-admin-center-developer-tools)是一个完全正常运行的扩展，可以是旁加载到 Windows Admin Center，并包含一系列丰富的可浏览和使用自己的扩展中的示例功能和工具示例。

## <a name="build-and-side-load-your-extension"></a>生成和端加载您的扩展插件

接下来，生成和端到 Windows Admin Center 加载你的扩展。  打开命令窗口，将目录更改为源目录，这样你就可以开始构建。

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

## <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>面向不同版本的 Windows Admin Center SDK

保持您的扩展插件的最新 SDK 更改和平台的更改非常简单。  阅读有关如何[面向不同版本](target-sdk-version.md)的 Windows Admin Center SDK。