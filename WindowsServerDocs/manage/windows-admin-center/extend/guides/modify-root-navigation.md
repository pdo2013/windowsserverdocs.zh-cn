---
title: 修改根导航行为
description: 开发解决方案扩展 Windows 管理中心 SDK （Project Honolulu）-修改根导航行为
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 08/07/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 78c94f3ea13f54ac31f9de9dd60873b93eba2c17
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385285"
---
# <a name="modify-root-navigation-behavior-for-a-solution-extension"></a>修改解决方案扩展的根导航行为

>适用于：Windows Admin Center、Windows Admin Center 预览版

在本指南中，我们将学习如何修改解决方案的根导航行为，使其具有不同的连接列表行为，以及如何隐藏或显示 "工具" 列表。

## <a name="modifying-root-navigation-behavior"></a>修改根导航行为

在 {extension root} \src 中打开 .manifest 文件，并找到属性 "rootNavigationBehavior"。 此属性有两个有效值： "连接" 或 "路径"。 本文稍后将详细介绍 "连接" 行为。

### <a name="setting-path-as-a-rootnavigationbehavior"></a>将路径设置为 rootNavigationBehavior

将 @no__t 值设置为 ```path```，然后删除 @no__t 属性，并将 ```path``` 属性保留为空字符串。 你已完成构建解决方案扩展所需的最小配置。 保存该文件，然后 > gulp gulp 服务，然后将该扩展加载到本地 Windows 管理中心扩展中。

有效的清单 s 数组如下所示：
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

使用此类结构生成的工具不需要连接来加载，但也不会有节点连接功能。

### <a name="setting-connections-as-a-rootnavigationbehavior"></a>将连接设置为 rootNavigationBehavior

将 ```rootNavigationBehavior``` 属性设置为 ```connections``` 时，会告诉 Windows 管理中心 Shell，会有一个连接的节点（始终是某种类型的服务器）应连接到该节点，并验证连接状态。 通过此操作，验证连接需要执行两个步骤。 1） Windows 管理中心将尝试使用您的凭据登录到该节点（用于建立远程 PowerShell 会话）和2），它将执行您提供的 PowerShell 脚本来确定节点是否处于可连接状态。

具有连接的有效解决方案定义将如下所示：

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

如果将 rootNavigationBehavior 设置为 "连接"，则需要构建清单中的连接定义。 这包括 "标头" 属性（当用户从菜单中选择它时，将用于在解决方案标题中显示）、connectionTypes 数组（这将指定在解决方案中使用的 connectionTypes。 有关详细信息，请参阅 connectionProvider 文档。）

## <a name="enabling-and-disabling-the-tools-menu"></a>启用和禁用 "工具" 菜单 ##

解决方案定义中可用的另一个属性是 "工具" 属性。 这会指示是否显示 "工具" 菜单以及要加载的工具。 启用后，Windows 管理中心将呈现左侧工具菜单。 使用 defaultTool，需要向清单添加工具入口点，以便加载相应的资源。 "DefaultTool" 的值必须是该工具在清单中定义的 "name" 属性。
