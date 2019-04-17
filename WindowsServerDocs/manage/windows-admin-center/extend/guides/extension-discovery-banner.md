---
title: 启用扩展发现横幅
description: 启用扩展发现横幅
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 03/08/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 389acba6d1fe6f65320bd780c9fde6469b387e0b
ms.sourcegitcommit: e558dda2774345e9ad17ff04b759f68c59d88139
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2019
ms.locfileid: "9262898"
---
# 启用扩展发现横幅 #

>适用于：Windows Admin Center、Windows Admin Center 预览版

在 Windows Admin Center 预览版 1903年中可用的新功能是扩展发现横幅。 此功能允许扩展声明的服务器硬件制造商和型号支持，并且当用户连接到服务器或群集扩展的可用，将显示通知横幅轻松安装扩展。 扩展开发人员将能够获取其扩展的更多的可见性和用户将能够轻松地了解更多的管理功能，用于其服务器。

![扩展发现横幅](../../media/extend-guides-extension-discovery-banner/extension-discovery-banner.png)

## 扩展发现横幅的工作原理 ##

启动 Windows Admin Center 时，它将连接到已注册的扩展源，并获取可用的扩展包的元数据。 然后当用户连接到服务器或群集 Windows Admin Center 中的，我们会读取概述工具中显示的服务器硬件制造商和型号。 如果我们找到声明它支持当前服务器的制造商和/或型号的扩展，我们将显示横幅以让用户知道。 单击"立即设置"链接将用户转到扩展管理器安装扩展。

## 如何实现扩展发现横幅 ##

.Nuspec 文件中的"标记"元数据用于声明的硬件制造商和/或模型扩展支持。 由空格分隔标记，你可以添加制造商或模型标记，或同时声明受支持的制造商和/或型号。 标记格式是``"[value type]_[value condition]"``其中 [值类型] 是一种的"制造商"或"模型"（区分大小写），而 [值条件] 是[Javascript 正则表达式](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)定义的制造商或模型字符串和 [值类型] 和 [值条件] 是用下划线分隔。 此字符串然后使用 URI 编码和添加到.nuspec"标记"元数据字符串进行编码。

### 示例 ###

我们假设我已开发扩展支持从公司模型名为 Contoso 公司的服务器名称 R3xx 和 R4xx。

1. 制造商的标记将``"Manufacturer_/Contoso Inc./"``。 模型的标记可能是``"Model_/^R[34][0-9]{2}$/"``。 具体取决于如何严格你想要定义匹配条件，将定义你正则表达式的不同方法。 此外可以制造商或模型标记分离到多个标记，例如，也可能是模型标记``"Model_/R3../ Model_/R4../"``。
2. 你可以测试 web 浏览器 DevTools 控制台的正则的表达式。 在 Edge 或者 Chrome，点击 F12 打开 DevTools 窗口中，并在控制台选项卡中，键入以下和点击 Enter:

```javascript
var regex = /^R[34][0-9]{2}$/
```

如果你键入并运行以下命令，它将返回 true。

```javascript
regex.test('R300')
```

如果你运行以下命令，它将返回 false。

```javascript
regex.test('R500')
```

3. 一旦你已验证的正则表达式，你可以对其进行编码在 DevTools 控制台中，使用以下 Javascript 方法：

```javascript
encodeURI(/^R[34][0-9]{2}$/)
```

要向.nuspec 文件中添加的标记字符串的最终格式是：

```
<tags>Manufacturer_/Contoso%20Inc./ Model_/%5ER%5B34%5D%5B0-9%5D%7B2%7D$/</tags>
```

> [!Tip]
> 我们知道硬件制造商可能有非常广泛的其中一些可能支持某些不很的型号名称。 请记住，此功能旨在帮助**发现**你的扩展，但它没有要所有模型完全保持最新清单。 你可以定义正则表达式要更简单的表达式的模型的子集相匹配。 用户可能不会看到发现横幅，如果他们首次连接到服务器模型不匹配条件，但迟早它们将连接到没有和将发现并安装扩展的另一台服务器。 你还可以考虑定义仅与你制造商的名称相匹配的简单正则表达式。 在某些情况下，你的扩展实际上可能不支持特定的模型，但你可以使用[动态工具显示功能](./dynamic-tool-display.md)来定义自定义 PowerShell 脚本来检查模型支持并仅显示你的扩展时适用，或提供限制对于不支持所有功能模型扩展中的功能。