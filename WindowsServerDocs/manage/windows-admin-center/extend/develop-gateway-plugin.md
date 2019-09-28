---
title: 开发网关插件
description: 开发网关插件 Windows 管理中心 SDK （Project Honolulu）
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 2b096b226190ad1ca3fd07c38b7b939d019ee30f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406933"
---
# <a name="develop-a-gateway-plugin"></a>开发网关插件

>适用于：Windows Admin Center、Windows Admin Center 预览版

Windows 管理中心网关插件启用从工具或解决方案的 UI 到目标节点的 API 通信。  Windows 管理中心托管一个网关服务，该服务可从要在目标节点上执行的网关插件中继命令和脚本。 网关服务可以进行扩展，以包括支持其他协议（默认值除外）的自定义网关插件。

默认情况下，在 Windows 管理中心中包括这些网关插件：

* PowerShell 网关插件
* WMI 网关插件

如果你想要使用 PowerShell 或 WMI 以外的协议进行通信，例如使用 REST，你可以构建自己的网关插件。  网关插件从现有网关进程加载到单独的 AppDomain，但对权限使用相同级别的提升。

> [!NOTE]
> 不熟悉不同的扩展类型？ 了解有关[扩展性体系结构和扩展类型](understand-extensions.md)的详细信息。

## <a name="prepare-your-environment"></a>准备你的环境

如果尚未准备好，请通过安装所有项目所需的依赖项和全局必备组件来[准备环境](prepare-development-environment.md)。

## <a name="create-a-gateway-plugin-c-library"></a>创建网关插件（C#库）

若要创建自定义网关插件，请C#从 @no__t 命名空间创建一个实现 ```IPlugIn``` 接口的新类。  

> [!NOTE]
> 在早期版本的 SDK 中可用的 @no__t 0 接口现在已标记为过时。  所有网关插件开发应使用 IPlugIn （或（可选） HttpPlugIn 抽象类）。

### <a name="download-sample-from-github"></a>从 GitHub 下载示例

若要快速开始使用自定义网关插件，你可以从 Windows 管理中心 SDK [GitHub 站点](https://aka.ms/wacsdk)克隆或下载我们的[ C#示例插件项目](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin)的副本。

### <a name="add-content"></a>添加内容

将新内容添加到[ C#示例插件项目](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin)项目（或您自己的项目）的克隆副本，以包含您的自定义 api，然后生成要在后续步骤中使用的自定义网关插件 DLL 文件。

### <a name="deploy-plugin-for-testing"></a>部署用于测试的插件

通过将自定义网关插件的 DLL 加载到 Windows 管理中心网关进程来对其进行测试。

Windows 管理中心在当前计算机的应用程序数据文件夹中查找 ```plugins``` 文件夹中的所有插件（使用 System.environment.specialfolder 枚举的 CommonApplicationData 值）。 在 Windows 10 上，此位置 ```C:\ProgramData\Server Management Experience```。  如果 ```plugins``` 文件夹尚不存在，你可以自行创建该文件夹。

> [!NOTE]
> 可以通过更新 "StaticsFolder" 配置值来覆盖调试版本中的插件位置。 如果正在本地调试，则此设置位于桌面解决方案的 app.config 中。 

在插件文件夹内（在此示例中，```C:\ProgramData\Server Management Experience\plugins```）

* 创建一个新文件夹，其名称与自定义网关插件 DLL 中 @no__t 的 ```Feature``` 属性值相同（在我们的示例项目中，```Name``` 为 "Sample Uno"）
* 将自定义网关插件 DLL 文件复制到此新文件夹
* 重新启动 Windows 管理中心进程

Windows 管理员进程重新启动后，你将能够通过发出 GET、PUT、PATCH、DELETE 或 POST 到 ```http(s)://{domain|localhost}/api/nodes/{node}/features/{feature name}/{identifier}```，在自定义网关插件 DLL 中执行 Api

### <a name="optional-attach-to-plugin-for-debugging"></a>可选：附加到用于调试的插件

在 Visual Studio 2017 的 "调试" 菜单中，选择 "附加到进程"。 在下一个窗口中，滚动到 "可用进程" 列表并选择 "SMEDesktop"，然后单击 "附加"。 调试器启动后，可以在功能代码中放置一个断点，然后演练上述 URL 格式。 对于我们的示例项目（功能名称："Sample Uno"） URL 是： "<http://localhost:6516/api/nodes/fake-server.my.domain.com/features/Sample%20Uno>"

## <a name="create-a-tool-extension-with-the-windows-admin-center-cli"></a>使用 Windows 管理中心 CLI 创建工具扩展 ##

现在，我们需要创建一个可在其中调用自定义网关插件的工具扩展。  创建或浏览到要在其中存储项目文件的文件夹，打开命令提示符，然后将该文件夹设置为工作目录。  使用之前安装的 Windows 管理中心 CLI 创建新扩展，并使用以下语法：

```
wac create --company "{!Company Name}" --tool "{!Tool Name}"
```

| ReplTest1 | 说明 | 示例 |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | 公司名称（包含空格） | ```Contoso Inc``` |
| ```{!Tool Name}``` | 工具名称（包含空格） | ```Manage Foo Works``` |

下面是一个示例用法：

```
wac create --company "Contoso Inc" --tool "Manage Foo Works"
```

这将使用为工具指定的名称在当前工作目录中创建一个新文件夹，将所有必需的模板文件复制到你的项目中，并使用你的公司和工具名称配置文件。  

接下来，将目录更改为刚创建的文件夹，然后通过运行以下命令安装所需的本地依赖项：

```
npm install
```

完成此操作后，已设置将新扩展加载到 Windows 管理中心所需的一切。 

## <a name="connect-your-tool-extension-to-your-custom-gateway-plugin"></a>将你的工具扩展连接到你的自定义网关插件

现在，你已使用 Windows 管理中心 CLI 创建了扩展，接下来可以执行以下步骤，将你的工具扩展连接到自定义网关插件：

- 添加[空模块](guides/add-module.md)
- 在工具扩展中使用[自定义网关插件](guides/use-custom-gateway-plugin.md)
 
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