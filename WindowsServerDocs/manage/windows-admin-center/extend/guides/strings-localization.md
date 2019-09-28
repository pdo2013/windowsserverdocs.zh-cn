---
title: Windows 管理中心中的字符串和本地化
description: 了解如何在 Windows 管理中心 SDK 中准备好用于本地化的字符串（Project Honolulu）
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 61289ae175ca8b906386cff9e36f5023ea28d051
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385236"
---
# <a name="strings-and-localization-in-windows-admin-center"></a>Windows 管理中心中的字符串和本地化 #

>适用于：Windows Admin Center、Windows Admin Center 预览版

让我们更深入地了解 Windows 管理中心扩展 SDK，并讨论字符串和本地化。

若要对呈现层上呈现的所有字符串启用本地化，请利用/src/resources/strings 下的 resjson 文件-它已设置好。 如果需要将新字符串添加到扩展，请将其作为新条目添加到此 resjson 文件。 现有结构的格式如下所示：

``` ts
"<YourExtensionName>_<Component>_<Accessor>": "Your string value goes here.",
```

您可以使用您喜欢的任何格式的字符串，但要注意，生成过程（采用 resjson 和输出可用 TypeScript 类的进程）将下划线（_）转换为句点（.）。

例如，以下条目：
``` ts
"HelloWorld_cim_title": "CIM Component",
```
生成以下访问器结构：
``` ts
MsftSme.resourcesStrings<Strings>().HelloWorld.cim.title;
```

## <a name="add-other-languages-for-localization"></a>添加其他本地化语言 ## 

对于其他语言的本地化，需要为每种语言创建 resjson 文件。 需要将这些文件放在中```\loc\output\{!ExtensionName}\{!LanguageFolder}\strings.resjson```。 具有相应文件夹的可用语言包括：

| 语言      | 文件夹      |
| ------------- |-------------|
| Čeština | cs-CZ |
| Deutsch | de-DE |
| 英语 | en-US |
| Español | es-ES |
| Français | fr-FR | 
| Magyar | hu-HU | 
| Italiano | it-IT |
| 日本語 | ja-JP | 
| 한국어 | ko-KR | 
| Nederlands | nl-NL |
| Polski | pl-PL |
| Português Brasileiro | pt-BR |
| Português (Portugal) | pt-PT |
| Русский | ru-RU |
| Svenska | sv-SE |
| Türkçe    | tr-TR |
| 中文(简体) | zh-CN |
| 中文(繁體) | zh-TW |
> [!NOTE]
> 如果文件结构需要在 "位置"/"输出" 内有所不同，则需要调整 gulpfile 中的 gulp 任务 "localeOffset" 的 resjson。 此偏移量是位置文件夹中应开始搜索 resjson 文件的深度。

每个 resjson 文件的格式设置方式与本指南顶部提及的格式相同。 

例如，若要包括西班牙语的本地化，请在中```\loc\output\HelloWorld\es-ES\strings.resjson```包含此项： 
```json
"HelloWorld_cim_title": "CIM Componente",
```
无论何时添加本地化字符串，都必须重新运行 gulp 才能使其显示。 运行：
``` cmd
gulp generate 
```

若要确认已生成字符串，请导航到```\src\app\assets\strings\{!LanguageFolder}\strings.resjson```。 新添加的条目将显示在此文件中。
现在，如果切换 Windows 管理中心中的 "语言" 选项，则可以在扩展中看到本地化的字符串。 
