---
title: 开发网关插件
description: 开发 Windows Admin Center SDK (项目 Honolulu) 的网关插件
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 66e36a349fc6bd38a77ccf4f00d380788ea4b422
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445952"
---
# <a name="develop-a-gateway-plugin"></a>开发网关插件

>适用于：Windows Admin Center，Windows Admin Center 预览版

Windows Admin Center 网关插件支持的 API 通信从用户界面的工具或解决方案到目标节点。  Windows Admin Center 托管中继命令和脚本从网关插件以在目标节点上执行的网关服务。 网关服务可以扩展以包括自定义网关插件的支持协议而不是默认值。

默认情况下与 Windows Admin Center 包括这些网关插件：

* PowerShell 网关插件
* WMI 网关插件

如果你想要使用 PowerShell 或 WMI 以外的协议进行通信，如使用 REST，你可以构建自己的网关插件。  网关插件加载到现有的网关进程，从单独的 AppDomain，但使用相同级别的提升的权限。

> [!NOTE]
> 不熟悉不同的扩展类型？ 详细了解如何[扩展性体系结构和扩展类型](understand-extensions.md)。

## <a name="prepare-your-environment"></a>准备你的环境

如果你尚未准备好，[准备您的环境](prepare-development-environment.md)通过安装依赖关系和所需的所有项目的全局系统必备组件。

## <a name="create-a-gateway-plugin-c-library"></a>创建网关插件 (C#库)

若要创建自定义网关插件，请创建一个新C#类，该类实现```IPlugIn```接口从```Microsoft.ManagementExperience.FeatureInterfaces```命名空间。  

> [!NOTE]
> ```IFeature```接口可在早期版本的 SDK，现在已标记为已过时。  所有网关插件开发应使用 IPlugIn （或 （可选） HttpPlugIn 抽象类）。

### <a name="download-sample-from-github"></a>从 GitHub 下载示例

若要快速开始自定义网关插件，可以克隆或下载一份我们[示例C#插件项目](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin)从我们的 Windows Admin Center SDK [GitHub 站点](https://aka.ms/wacsdk)。

### <a name="add-content"></a>添加内容

将新内容添加到克隆的副本[示例C#插件项目](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin)要在后续步骤中使用的项目 （或你自己的项目） 包含自定义 Api，然后生成自定义网关插件 DLL 文件。

### <a name="deploy-plugin-for-testing"></a>部署适用于测试插件

通过加载到 Windows Admin Center 网关进程来测试你的自定义网关插件 DLL。

Windows Admin Center 将查找所有插件```plugins```文件夹中 （使用 Environment.SpecialFolder 枚举 CommonApplicationData 值） 在当前计算机的应用程序数据文件夹。 此位置是 Windows 10 上```C:\ProgramData\Server Management Experience```。  如果```plugins```文件夹不存在，但您可以自己创建的文件夹。

> [!NOTE]
> 您可以通过更新"StaticsFolder"配置值来覆盖在调试版本的插件位置。 如果在本地进行调试，此设置是在 App.Config 文件中的桌面解决方案。 

插件文件夹内 (在此示例中， ```C:\ProgramData\Server Management Experience\plugins```)

* 使用相同的名称创建一个新的文件夹```Name```属性值为```Feature```自定义网关插件 DLL 中 (在我们的示例项目，```Name```是"示例 Uno")
* 将你的自定义网关插件 DLL 文件复制到此新文件夹
* 重新启动 Windows Admin Center 进程

Windows 管理员进程重新启动后，你将能够执行自定义网关插件 DLL 中的 Api 发出 GET、 PUT、 PATCH、 DELETE 或发布到 ```http(s)://{domain|localhost}/api/nodes/{node}/features/{feature name}/{identifier}```

### <a name="optional-attach-to-plugin-for-debugging"></a>可选：将附加到适用于调试插件

在 Visual Studio 2017 中，从调试菜单中，选择"附加到进程"。 在下一步的窗口中，可用进程列表中滚动，选择 SMEDesktop.exe，然后单击"附加"。 一次调试器将启动，可以将断点功能代码和更高版本的 URL 格式通过然后练习中放置。 对于我们的示例项目 (功能名称："示例 Uno") 的 URL 是:"<http://localhost:6516/api/nodes/fake-server.my.domain.com/features/Sample%20Uno>"

## <a name="create-a-tool-extension-with-the-windows-admin-center-cli"></a>使用 Windows Admin Center CLI 中创建的工具扩展 ##

现在，我们需要创建可以从其调用你的自定义网关插件的工具扩展。  创建或浏览到用于存储项目文件，打开命令提示符，并为工作目录设置该文件夹的文件夹。  使用之前安装 Windows Admin Center CLI，使用以下语法创建一个新的扩展：

```
wac create --company "{!Company Name}" --tool "{!Tool Name}"
```

| ReplTest1 | 说明 | 示例 |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | 你的公司名称 （包含空格） | ```Contoso Inc``` |
| ```{!Tool Name}``` | 你的工具名称 （包含空格） | ```Manage Foo Works``` |

下面是一个示例用法：

```
wac create --company "Contoso Inc" --tool "Manage Foo Works"
```

这将创建使用您所需的工具为指定的名称，将所有所需的模板文件复制到项目中，使用你的公司和工具名称配置文件的当前工作目录中的新文件夹。  

接下来，将目录更改为刚创建的文件夹，然后通过运行以下命令安装所需的本地依赖项：

```
npm install
```

完成此操作，你已设置所有需要在新的扩展加载到 Windows Admin Center。 

## <a name="connect-your-tool-extension-to-your-custom-gateway-plugin"></a>连接到你的自定义网关插件的工具扩展

现在，已使用 Windows Admin Center CLI 创建一个扩展，你已准备好连接到你的自定义网关插件，您的工具扩展，通过执行以下步骤：

- 添加[空模块](guides/add-module.md)
- 使用你[自定义网关插件](guides/use-custom-gateway-plugin.md)工具扩展中
 
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