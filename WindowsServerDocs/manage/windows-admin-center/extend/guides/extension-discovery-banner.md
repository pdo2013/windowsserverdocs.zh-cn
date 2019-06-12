---
title: 启用扩展发现横幅
description: 启用扩展发现横幅
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: fafc16d6acae3c5a7a58a379d2f88998b8e98c3d
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811877"
---
# <a name="enabling-the-extension-discovery-banner"></a>启用扩展发现横幅

>适用于：Windows Admin Center，Windows Admin Center 预览版

Windows Admin Center 预览版 1903年中提供的新功能是扩展发现横幅。 此功能允许以声明的服务器硬件制造商提供和支持，模型的扩展，当用户连接到服务器或群集扩展为进行时，将显示通知横幅可以轻松地安装该扩展。 扩展开发人员将能够为其扩展获取更多可见性和用户将能够轻松发现其服务器的多个管理功能。

![扩展发现横幅](../../media/extend-guides-extension-discovery-banner/extension-discovery-banner.png)

## <a name="how-the-extension-discovery-banner-works"></a>扩展发现横幅的工作原理

启动 Windows Admin Center 时，它将连接到已注册的扩展源并提取可用扩展包的元数据。 然后，当用户连接到服务器或 Windows Admin Center 中的群集，我们读取概述工具中显示的服务器硬件制造商和型号。 如果我们找到声明它支持当前服务器的制造商和/或模型的扩展，我们将显示一个横幅，让用户知道。 单击"立即设置"链接将用户转到扩展管理器，用户可从中安装该扩展。

## <a name="how-to-implement-the-extension-discovery-banner"></a>如何实现扩展发现横幅

.Nuspec 文件中的"tags"元数据用于声明哪些硬件制造商和/或模型扩展插件支持。 由空格分隔的标记和您可以添加制造商或模型标记，或同时以声明的受支持的制造商和/或模型。 标记格式``"[value type]_[value condition]"``其中 [值类型] 为"制造商"模型"（区分大小写），，[值条件] 是[Javascript 正则表达式](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)定义制造商或模型字符串和 [值类型] 和 [值条件] 以下划线分隔。 然后，此字符串被编码使用编码和添加到.nuspec"tags"元数据字符串的 URI。

### <a name="example"></a>示例

假设我开发了一个扩展，支持从模型创建名为 Contoso Inc.的公司服务器 R3xx 和 R4xx 命名。

1. 制造商的标记将``"Manufacturer_/Contoso Inc./"``。 模型标记可以是``"Model_/^R[34][0-9]{2}$/"``。 具体取决于严格程度，你想要定义的匹配条件，将有多种方法可以定义正则表达式。 此外可以将制造商或型号标记分到多个标记，例如，模型标记也可能是``"Model_/R3../ Model_/R4../"``。
2. 你可以测试正则表达式和 web 浏览器的 DevTools 控制台。 在 Edge 或 Chrome 中，按 F12 打开 DevTools 窗口中，并在控制台选项卡中，键入以下并点击 enter 键：

   ```javascript
   var regex = /^R[34][0-9]{2}$/
   ```

   如果键入，然后运行以下命令，则会返回 true。

   ```javascript
   regex.test('R300')
   ```

   如果你运行以下命令，它将返回 false。

   ```javascript
   regex.test('R500')
   ```

3. 一旦你已验证的正则表达式，您可以对其进行编码在 DevTools 控制台中，使用以下 Javascript 方法：

   ```javascript
   encodeURI(/^R[34][0-9]{2}$/)
   ```

   要添加到你的.nuspec 文件的标记字符串的最终格式是：

   ```
   <tags>Manufacturer_/Contoso%20Inc./ Model_/%5ER%5B34%5D%5B0-9%5D%7B2%7D$/</tags>
   ```

> [!Tip]
> 我们了解到，硬件制造商可能具有非常广泛的其中一些可能受支持时，某些不是模型名称。 请注意，此功能旨在帮助**发现**的扩展，但它并没有为所有模型的完全保持最新清单。 您可以定义正则表达式是更简单的表达式的子集的模型相匹配。 用户可能看不到发现横幅如果他们首先连接到不匹配条件，但服务器模型，但还是迟早它们将连接到另一台服务器，没有和将发现并安装该扩展。 您还可以考虑定义简单的正则表达式仅匹配制造商名称。 在某些情况下，扩展实际上可能不支持特定的模型，但你可以使用[动态工具显示功能](./dynamic-tool-display.md)来定义自定义 PowerShell 脚本，以检查模型的支持，并仅显示你的扩展时适用，或对于不支持所有功能的模型提供有限的功能，在您的扩展插件。