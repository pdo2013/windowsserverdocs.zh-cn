---
title: 开发工具扩展
description: 开发工具扩展 Windows Admin Center SDK (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 7dd213f7032ab77021bbfcbdc966c9c2307b86eb
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/20/2018
ms.locfileid: "4081054"
---
# 开发工具扩展

>适用于：Windows Admin Center、Windows Admin Center 预览版

工具扩展是用户与 Windows Admin Center 管理的连接，如服务器或群集交互的主要方式。 在单击 Windows Admin Center 主屏幕中的连接并连接时，你将再看到的左侧的导航窗格中的工具列表。 当你单击工具时，工具扩展可以加载，并显示在右侧窗格中。

工具扩展加载时，它可以在目标服务器或群集上执行 WMI 调用或 PowerShell 脚本，并在 UI 中显示信息或执行命令基于用户输入。 工具扩展定义它应会显示的解决方案，导致一组不同的每个解决方案的工具。

> [!NOTE]
> 不熟悉的不同扩展类型？ 了解有关[扩展性体系结构和扩展类型](understand-extensions.md)的更多信息。

## 准备你的环境

如果你尚未，通过安装依赖项和所需的所有项目的全局先决条件[准备你的环境](prepare-development-environment.md)。

## 使用 Windows Admin Center CLI 创建一个新的工具扩展 ##

安装的所有依赖项后，你可以随时创建新的工具扩展。  创建或浏览到包含项目文件的文件夹、 打开命令提示符，并将该文件夹设置作为工作目录。  使用 Windows Admin Center CLI 之前已安装，使用以下语法创建新的扩展：

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

## 将内容添加到你的扩展

既然你已使用 Windows Admin Center CLI 创建扩展，你就可以自定义内容。  有关可以执行哪些操作的示例，请参阅以下指南：

- 添加一个[空模块](guides\add-module.md)
- 添加[iFrame](guides\add-iframe.md)
 
我们的[GitHub SDK 站点](https://aka.ms/wacsdk)，可以找到更多示例：
-  [开发人员工具](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/windows-admin-center-developer-tools)是一个完全正常运行的扩展，可以旁加载到 Windows Admin Center，其中包含丰富的示例功能和工具示例，你可以中浏览和使用你自己的扩展。

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