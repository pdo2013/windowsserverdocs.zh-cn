---
title: 修改根导航行为
description: '制定解决方案扩展 Windows Admin Center SDK (Project Honolulu): 修改根导航行为'
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 08/07/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 4a5cba228aa3a0afed99c0d853c3720a5b46f650
ms.sourcegitcommit: 546229d6b5fa7e16f725c6c35f4dcc272711b811
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/18/2018
ms.locfileid: "4905037"
---
# 修改解决方案扩展的根导航行为

>适用于：Windows Admin Center、Windows Admin Center 预览版

在本指南中，我们将了解如何修改你的解决方案具有不同的连接列表行为，请的根导航行为，以及如何隐藏或显示工具列表。

## 修改根导航行为

打开中 {扩展根} \src manifest.json 文件并查找"rootNavigationBehavior"的属性。 此属性具有两个有效的值:"连接"或"路径"。 "连接"行为将在文档的后面部分详细介绍。

### 设置路径作为 rootNavigationBehavior

设置的值```rootNavigationBehavior```到```path```，然后删除```requirements```属性，然后将```path```属性为空字符串。 你已完成的最小的所需的配置来构建解决方案扩展。 保存该文件，并 gulp 生成-> gulp 用作你将工具，然后旁加载扩展到本地 Windows Admin Center 扩展。

有效清单的入口点数组如下所示：
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

这种结构与内置工具将不是必需的连接，若要加载，但也不会获得节点连接功能。

### 将连接设置为 rootNavigationBehavior

当你设置```rootNavigationBehavior```属性```connections```，会告诉 Windows Admin Center Shell 将连接的节点 （始终某些类型的服务器），则应连接，并验证连接状态。 与此，需要两个步骤验证连接。 1) Windows Admin Center 将尝试尝试登录到与你的凭据 （用于建立远程 PowerShell 会话），节点并 2） 将执行 PowerShell 脚本你提供用来标识该节点是否处于连接状态。

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

当 rootNavigationBehavior 设置为"连接"你需要在清单中的连接定义构建。 这包括"标头"属性 （将用于显示你的解决方案标头中，当用户从菜单中选择）、 connectionTypes 数组 （这将指定哪些 connectionTypes 解决方案中使用。 详细信息的一样文档中。）。

## 启用和禁用工具菜单 ##

在解决方案定义中可用的另一个属性是"工具"属性。 这将决定是否显示工具菜单，并将加载该工具。 启用后，Windows Admin Center 将呈现左侧的工具菜单。 对于 defaultTool，它需要进行为了加载合适的资源将工具入口点添加到清单。 "DefaultTool"的值必须是该工具的"名称"属性，它是在清单中定义。
