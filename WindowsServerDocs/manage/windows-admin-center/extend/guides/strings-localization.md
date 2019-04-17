---
title: 字符串和 Windows Admin Center 中的本地化
description: 了解如何在 Windows Admin Center SDK (Project Honolulu) 中获取你的字符串为本地化准备就绪
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: f0671160bd790d80e35f6d6c1586a07969939070
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296729"
---
# 字符串和 Windows Admin Center 中的本地化 #

>适用于：Windows Admin Center、Windows Admin Center 预览版

让我们更深入地了解到 Windows Admin Center 扩展 SDK 还能讨论字符串和本地化。

若要启用的表示层，充分利用下 /src/resources/strings-strings.resjson 文件呈现的所有字符串的本地化它已经设置。 当你需要将新的字符串添加到你的扩展时，将其添加到此 resjson 文件作为新项。 现有的结构遵循此格式：

``` ts
"<YourExtensionName>_<Component>_<Accessor>": "Your string value goes here.",
```

可以使用你喜欢的任何格式的字符串，但请注意生成过程 （获取 resjson 并输出可用 TypeScript 类的进程） 将下划线 (_) 转换为句点 （.）。

例如，此条目：
``` ts
"HelloWorld_cim_title": "CIM Component",
```
生成以下访问器结构：
``` ts
MsftSme.resourcesStrings<Strings>().HelloWorld.cim.title;
```

## 添加其他语言进行本地化 ## 

对于为其他语言的本地化，需要为每种语言创建 strings.resjson 文件。 这些文件需要位置```\loc\output\{!ExtensionName}\{!LanguageFolder}\strings.resjson```。 使用相应的文件夹中的可用语言是：

| 语言      | 文件夹      |
| ------------- |-------------|
| Čeština | cs-CZ |
| 将德语 | de-DE |
| 英语 | en-US |
| 尽管 | es-ES |
| Français | fr-FR | 
| Magyar | hu-HU | 
| 意大利语 | it-IT |
| 日本語 | ja-JP | 
| 한국어 | ko-KR | 
| 荷兰 | nl-NL |
| Polski | pl-PL |
| Português (Brasil) | pt-BR |
| Português （葡萄牙） | pt-PT |
| РУССКИЙ | ru-RU |
| Svenska | sv-SE |
| Türkçe    | tr-TR |
| 中文(简体) | zh-CN |
| 中文(繁體) | zh-TW |
> [!NOTE]
> 如果你的文件结构需要本地化/输出的不同内，你将需要调整 gulp 任务生成 resjson 的 json-本地化正在 gulpfile.js localeOffset。 此偏移量是如何深入了解它应开始搜索 strings.resjson 文件的本地化文件夹。

每个 strings.resjson 文件都将在本指南的顶部如之前所述的相同方式设置格式。 

例如，要包括的西班牙语本地化将此条目包含在```\loc\output\HelloWorld\es-ES\strings.resjson```: 
```json
"HelloWorld_cim_title": "CIM Componente",
```
随时添加本地化的字符串、 gulp 生成 must 为了使其看上去再次运行。 运行：
``` cmd
gulp generate 
```

若要确认已生成你的字符串导航到```\src\app\assets\strings\{!LanguageFolder}\strings.resjson```。 在新添加的项将显示在此文件。
现在如果切换 Windows Admin Center 中的语言选项，你将能够看到你的扩展中的本地化的字符串。 
