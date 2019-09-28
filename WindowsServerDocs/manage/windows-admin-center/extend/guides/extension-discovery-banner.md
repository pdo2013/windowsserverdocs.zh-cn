---
title: 启用扩展发现横幅
description: 启用扩展发现横幅
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: fb549d84f565feeea348d2f50a9188218e7638d1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357075"
---
# <a name="enabling-the-extension-discovery-banner"></a>启用扩展发现横幅

>适用于：Windows Admin Center、Windows Admin Center 预览版

Windows 管理中心预览1903中提供的一项新功能是扩展发现横幅。 此功能允许扩展声明服务器硬件制造商和它所支持的模型，并且当用户连接到其扩展插件可用的服务器或群集时，将显示一条通知横幅以便轻松安装该扩展。 扩展开发人员将能够更好地了解其扩展，用户可以轻松地发现其服务器的更多管理功能。

![扩展发现横幅](../../media/extend-guides-extension-discovery-banner/extension-discovery-banner.png)

## <a name="how-the-extension-discovery-banner-works"></a>扩展发现横幅的工作方式

启动 Windows 管理中心后，它将连接到已注册的扩展源，并提取可用扩展包的元数据。 然后，当用户连接到 Windows 管理中心中的某个服务器或群集时，我们会阅读服务器硬件制造商和要在概述工具中显示的模型。 如果找到一个声明支持当前服务器的制造商和/或型号的扩展，我们将显示一个横幅，告诉用户。 单击 "立即设置" 链接可将用户转到扩展管理器，用户可以在其中安装扩展。

## <a name="how-to-implement-the-extension-discovery-banner"></a>如何实现扩展发现横幅

Nuspec 文件中的 "标记" 元数据用于声明你的扩展所支持的硬件制造商和/或型号。 标记由空格分隔，你可以添加制造商或型号标记，或者同时添加这两个标记以声明受支持的制造商和/或模型。 标记格式为 ``"[value type]_[value condition]"``，其中 [值类型] 为 "制造商" 或 "模型" （区分大小写），而 "值条件" 是定义制造商或模型字符串的[Javascript 正则表达式](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)，而 "值类型" 和 "值条件" 被分隔下划线。 然后使用 URI 编码对该字符串进行编码，并将其添加到 nuspec "tags" 元数据字符串中。

### <a name="example"></a>示例

假设我开发了一个扩展，它支持名为 Contoso Inc. 的公司的服务器，模型名称为 R3xx 和 R4xx。

1. 制造商的标记将 ``"Manufacturer_/Contoso Inc./"``。 模型的标记可以 ``"Model_/^R[34][0-9]{2}$/"``。 根据要定义匹配条件的严格程度，可以通过不同的方式来定义正则表达式。 你还可以将制造商或型号标记分成多个标记，例如，模型标记也可以 ``"Model_/R3../ Model_/R4../"``。
2. 可以通过 web 浏览器的 DevTools 控制台来测试正则表达式。 在 "边缘" 或 "Chrome" 中，按 F12 打开 "DevTools" 窗口，并在 "控制台" 选项卡中键入以下内容并按 Enter：

   ```javascript
   var regex = /^R[34][0-9]{2}$/
   ```

   然后，如果键入并运行以下项，则将返回 "true"。

   ```javascript
   regex.test('R300')
   ```

   如果运行以下方法，则它将返回 "false"。

   ```javascript
   regex.test('R500')
   ```

3. 验证正则表达式后，还可以使用以下 Javascript 方法在 DevTools 控制台中对其进行编码：

   ```javascript
   encodeURI(/^R[34][0-9]{2}$/)
   ```

   要添加到 nuspec 文件的标记字符串的最终格式为：

   ```
   <tags>Manufacturer_/Contoso%20Inc./ Model_/%5ER%5B34%5D%5B0-9%5D%7B2%7D$/</tags>
   ```

> [!Tip]
> 我们了解到，硬件制造商可能有很多型号的名称，其中某些模型名称可能受支持，而有些则不受支持。 请记住，此功能旨在帮助**发现**你的扩展，但它不一定是你的所有模型的最新清单。 您可以将正则表达式定义为与模型子集相匹配的更简单的表达式。 如果用户第一次连接到与条件不匹配的服务器模型，则该用户可能看不到发现横幅，但更早或更高的用户将连接到另一台服务器，该服务器将发现并安装该扩展。 你还可以考虑定义仅与制造商名称匹配的简单正则表达式。 在某些情况下，你的扩展可能并不实际支持特定的模型，但你可以使用[动态工具显示功能](./dynamic-tool-display.md)来定义自定义 PowerShell 脚本以检查模型支持，并仅在适用时显示你的扩展，或提供有限扩展中的功能，适用于不支持所有功能的模型。