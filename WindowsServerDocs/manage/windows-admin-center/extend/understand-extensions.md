---
title: 了解 Windows Admin Center 扩展
description: 了解 Windows Admin Center SDK 扩展 (Project Honolulu)
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 4bfabe4959fe16f5e240cbf1a972a902e37ffb52
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385255"
---
# <a name="understanding-windows-admin-center-extensions"></a>了解 Windows Admin Center 扩展

>适用于：Windows Admin Center、Windows Admin Center 预览版

如果你尚不熟悉 Windows 管理中心中心的工作方式，让我们从高级体系结构开始。 Windows Admin Center 由两个主要组件构成：

- 轻型 **Web 服务**，向 Web 浏览器请求提供 Windows Admin Center UI Web 页面。
- **网关组件**，侦听来自 Web 页面的 REST API 请求并中继要在目标服务器或群集上执行的 WMI 调用或 PowerShell 脚本。

![Windows Admin Center 体系结构](../media/understand-extensions/wac-architecture-500px.png)

Web 服务提供的 Windows Admin Center UI Web 页面从可扩展性角度有两个主要 UI 组件：解决方案和工具，其作为扩展实现，以及第三个称为网关插件的扩展类型。

## <a name="solution-extensions"></a>解决方案扩展

在 Windows Admin Center 主屏幕中，默认情况下，你可以添加四个类型之一的连接 - Windows Server 连接、Windows 电脑连接、故障转移群集连接和超融合群集连接。 添加连接后，将在主屏幕中显示连接名称和类型。 单击连接名称将尝试连接到目标服务器或群集，然后加载连接的 UI。

![Windows Admin Center 体系结构](../media/understand-extensions/solutions-ui.png)

这个连接类型的每个类型均映射到一个解决方案，解决方案通过“解决方案”扩展扩展类型定义。 解决方案通常定义你希望通过 Windows Admin Center 管理的唯一对象类型，如服务器、电脑或故障转移群集。 你还可以定义用于连接到并管理其他设备（如网络交换机和 Linux 服务器）的新解决方案，甚至是像远程桌面服务这样的服务。

## <a name="tool-extensions"></a>工具扩展

在单击 Windows Admin Center 主屏幕中的连接并连接时，所选连接类型的解决方案扩展将加载，然后将向你呈现解决方案 UI，并在左侧导航窗格中显示工具列表。 在你单击工具时，工具 UI 将加载并在右侧窗格中显示。

![Windows Admin Center UI 体系结构](../media/understand-extensions/ui-architecture.png)

这些工具中的每一个均通过第二个扩展类型“工具”扩展定义。 在加载工具时，它可以在目标服务器或群集上执行 WMI 调用或 PowerShell 脚本，并可以基于用户输入在 UI 中显示信息或执行命令。 工具扩展定义它应为其显示的解决方案，因此每个解决方案的工具集会不同。 如果你在创建新的解决方案扩展，你额外还需要编写一个或多个为解决方案提供功能的工具扩展。

![每个解决方案的工具列表](../media/understand-extensions/tools-for-solutions.png)

## <a name="gateway-plugins"></a>网关插件

网关服务公开 UI 的 REST API 来调用和中继要在目标上执行的命令和脚本。 网关服务可以由支持不同协议的网关插件扩展。 Windows Admin Center 预打包了两个网关插件，一个用于执行 PowerShell 脚本，另一个用于执行 WMI 命令。 如果你需要通过 PowerShell 或 WMI 以外的协议（如 REST）与目标通信，则可以为此构建网关插件。

## <a name="next-steps"></a>后续步骤

根据你想要在 Windows Admin Center 中构建的功能，为现有服务器或群集解决方案[构建工具扩展](develop-tool.md)可能已经足够，并且是构建扩展最简单的第一步。 但是，如果你的功能是用于管理设备、服务或一些全新内容，而不是服务器或群集，则应考虑使用一个或多个工具[构建解决方案扩展](develop-solution.md)。 最后，如果需要通过 WMI 或 PowerShell 以外的协议与目标通信，则需要[构建网关插件](develop-gateway-plugin.md)。 [继续阅读](developing-extensions.md)，了解如何设置开发环境以及开始编写你的第一个扩展。
