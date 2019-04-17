---
title: 开发网关插件
description: 开发网关插件 Windows Admin Center SDK (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 93cee5b8e3611a264119947103d22d9aa3b9a56b
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/20/2018
ms.locfileid: "4081154"
---
# 开发网关插件

>适用于：Windows Admin Center、Windows Admin Center 预览版

Windows Admin Center 网关插件支持 API 通信从 UI 工具或解决方案的到目标节点。  Windows Admin Center 托管中继要在目标节点上执行的命令和脚本从网关插件的网关服务。 网关服务可以进行扩展以包括支持默认模板以外的协议的自定义的网关插件。

默认情况下，使用 Windows Admin Center 包含这些网关插件：

* PowerShell 网关插件
* WMI 网关插件

如果你想要使用 PowerShell 或 WMI 以外的协议进行通信，例如与其余部分，你可以构建网关插件。  网关插件加载到不同的 AppDomain 从现有的网关进程，但使用相同级别的提升的权限。

> [!NOTE]
> 不熟悉的不同扩展类型？ 了解有关[扩展性体系结构和扩展类型](understand-extensions.md)的更多信息。

## 准备你的环境

如果你尚未，通过安装依赖项和所需的所有项目的全局先决条件[准备你的环境](prepare-development-environment.md)。

## 创建一个网关插件 （C# 库）

若要创建一个自定义的网关插件，创建一个新 C# 类实现```IPlugIn```接口从```Microsoft.ManagementExperience.FeatureInterfaces```命名空间。  

> [!NOTE]
> ```IFeature```接口，可在早期版本的 SDK，现在已标记为已过时。  所有网关插件开发应使用 IPlugIn （或选择性地 HttpPlugIn 抽象类）。

### 从 GitHub 下载示例

若要快速开始自定义的网关插件，可以克隆或下载我们的[示例 C# 插件项目](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin)一份从我们 Windows Admin Center SDK [GitHub 站点](https://aka.ms/wacsdk)。

### 添加内容

将新内容添加到克隆副本[项目示例 C# 插件](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin)项目 （或你自己的项目） 来包含你的自定义 Api，则生成你的自定义的网关插件 DLL 文件，以在后续步骤中使用。

### 部署用于测试的插件

测试你的自定义的网关插件 DLL 加载到 Windows Admin Center 网关进程。

Windows Admin Center 查找所有插件```plugins```当前的计算机 （使用 Environment.SpecialFolder 枚举的 CommonApplicationData 值） 的应用程序数据文件夹中的文件夹。 Windows 10 上的此位置是```C:\ProgramData\Server Management Experience```。  如果```plugins```文件夹尚不存在，可以自行创建该文件夹。

> [!NOTE]
> 你可以通过更新的"StaticsFolder"配置值替代在调试版本的插件位置。 如果本地调试时，此设置为中的桌面解决方案的控件。 

在插件文件夹内 (在此示例中， ```C:\ProgramData\Server Management Experience\plugins```)

* 使用相同的名称创建一个新文件夹```Name```属性值为```Feature```在你的自定义的网关插件 DLL (在我们的示例项目，```Name```是"示例 Uno")
* 将自定义的网关插件 DLL 文件复制到此新文件夹
* 重新启动 Windows Admin Center 过程

Windows Admin 过程重启后，你将能够通过发出获取练习自定义的网关插件 DLL 中的 Api 放入、 修补程序、 删除或发布到 ```http(s)://{domain|localhost}/api/nodes/{node}/features/{feature name}/{identifier}```

### 可选： 将附加到的调试插件

在 Visual Studio 2017 中，从调试菜单中，选择"附加到进程"。 在下一步的窗口中，滚动可用进程列表并选择 SMEDesktop.exe，然后单击"连接"。 一次在调试程序开始时，你可以放置一个断点功能代码和然后练习中，通过上述 URL 格式。 有关我们的示例项目 (功能名称:"示例 Uno") 的 URL 是:"http://localhost:6516/api/nodes/fake-server.my.domain.com/features/Sample%20Uno"

## 使用 Windows Admin Center CLI 创建工具扩展 ##

现在，我们需要创建工具扩展，你可以从中调用你的自定义的网关插件。  创建或浏览到想要存储项目文件、 打开命令提示符，并将该文件夹设置作为工作目录的文件夹。  使用 Windows Admin Center CLI 之前已安装，使用以下语法创建新的扩展：

```
wac create --company "{!Company Name}" --tool "{!Tool Name}"
```

| 值 | 说明 | 示例 |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | 你的公司名称 （使用空格） | ```Contoso Inc``` |
| ```{!Tool Name}``` | 你的工具名称 （使用空格） | ```Manage Foo Works``` |

下面是一个示例用法：

```
wac create --company "Contoso Inc" --tool "Manage Foo Works"
```

这将创建一个新文件夹内使用的名称指定为你的工具、 将所有所需的模板文件复制到你的项目，并使用你的公司和工具名称来配置文件的当前工作目录。  

接下来，将目录更改到刚创建的文件夹，然后通过运行以下命令安装所需的本地依赖项：

```
npm install
```

这完成后，你已设置所有需要加载到 Windows Admin Center 的新扩展。 

## 连接到你的自定义的网关插件的工具扩展

既然你已使用 Windows Admin Center CLI 创建扩展，你可以随时连接到你的自定义的网关插件，你的工具扩展，按照以下步骤：

- 添加一个[空模块](guides\add-module.md)
- 工具扩展中使用你的[自定义的网关插件](guides\use-custom-gateway-plugin.md)
 
## 构建并旁加载你的扩展

接下来，构建并旁加载你的扩展到 Windows Admin Center。  打开命令窗口，将目录更改为你的源目录，然后便可以生成。

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

## 目标是不同版本的 Windows Admin Center SDK

使你的扩展 SDK 更改和平台的更改保持最新很容易。  阅读有关如何为[目标的不同版本](target-sdk-version.md)的 Windows Admin Center SDK。