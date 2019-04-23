---
title: 修改根导航行为
description: 开发解决方案扩展 Windows Admin Center SDK (项目 Honolulu)-修改根导航行为
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 08/07/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 4a5cba228aa3a0afed99c0d853c3720a5b46f650
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861728"
---
# <a name="modify-root-navigation-behavior-for-a-solution-extension"></a>修改解决方案扩展根导航行为

>适用于：Windows Admin Center，Windows Admin Center 预览版

在本指南中我们将了解如何修改你的解决方案具有不同的连接列表的行为，根导航行为，以及如何隐藏或显示的工具列表。

## <a name="modifying-root-navigation-behavior"></a>修改根导航行为

在 {扩展根目录} \src 打开 manifest.json 文件并找到"rootNavigationBehavior"的属性。 此属性具有两个有效的值:"连接"或"路径"。 更高版本的文档中详细说明了"连接"行为。

### <a name="setting-path-as-a-rootnavigationbehavior"></a>作为 rootNavigationBehavior 设置路径

设置的值```rootNavigationBehavior```到```path```，然后删除```requirements```属性，然后让```path```属性为空字符串。 已完成最小所需的配置，以生成解决方案扩展。 保存该文件，和 gulp 生成-> gulp 作为您将工具，然后端加载扩展到本地 Windows Admin Center 扩展中。

一个有效的清单入口点数组，如下所示：
```
    "entryPoints": [
        {
          "entryPointType": "solution",
          "name": "main",
          "urlName": "testsln",
          "displayName": "resources:strings:displayName",
          "description": "resources:strings:description",
          "icon": "sme-icon:icon-win-powerShell",
          "path": "",
          "rootNavigationBehavior": "path"
        }
    ],
```

使用此类结构构建的工具将不是必需的连接，若要加载，但也不会具有节点的连接功能。

### <a name="setting-connections-as-a-rootnavigationbehavior"></a>将连接设置为 rootNavigationBehavior

当您将设置```rootNavigationBehavior```属性设置为```connections```，您将指示 Windows Admin Center Shell 将作为已连接的节点 （始终某些类型的服务器），它应连接到，并验证连接状态。 使用此操作，请验证连接中有 2 个步骤。 1) Windows Admin Center 将尝试使尝试登录到你的凭据 （用于建立远程 PowerShell 会话），使用节点和 2） 它将执行您提供用来确定节点是否处于可连接状态的 PowerShell 脚本。

连接的有效的解决方案定义将如下所示：

``` json
        {
          "entryPointType": "solution",
          "name": "example",
          "urlName": "solutionexample",
          "displayName": "resources:strings:displayName",
          "description": "resources:strings:description",
          "icon": "sme-icon:icon-win-powerShell",
          "rootNavigationBehavior": "connections",
          "connections": {
            "header": "resources:strings:connectionsListHeader",
            "connectionTypes": [
                "msft.sme.connection-type.example"
                ]
            },
            "tools": {
                "enabled": false,
                "defaultTool": "solution"
            }
        },
```

RootNavigationBehavior 设置为"连接"。 需要扩建清单中的连接定义。 这包括"标头"属性 （将用于当用户从菜单中选择解决方案标头中显示）、 connectionTypes 数组 （这将指定哪些 connectionTypes 解决方案中使用。 对此做详细 connectionProvider 文档中。）。

## <a name="enabling-and-disabling-the-tools-menu"></a>启用和禁用工具菜单 ##

解决方案定义中提供的另一个属性是"工具"属性。 这将决定是否显示工具菜单，并将加载该工具。 启用时，Windows Admin Center 将呈现左侧的工具菜单。 借助 defaultTool，就需要为加载相应的资源将工具入口点添加到清单。 "DefaultTool"的值必须是该工具的"name"属性，因为它在清单中定义。
