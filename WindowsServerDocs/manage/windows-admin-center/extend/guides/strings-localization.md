---
title: 字符串和本地化中 Windows Admin Center
description: 了解如何获取您的字符串 Windows Admin Center SDK (项目 Honolulu) 中准备好进行本地化
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: fb328f86c98141a5a2a1c4fd05ec1d4c96a7bc5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845398"
---
# <a name="strings-and-localization-in-windows-admin-center"></a>字符串和本地化中 Windows Admin Center #

>适用于：Windows Admin Center，Windows Admin Center 预览版

让我们更深入到 Windows Admin Center 扩展 SDK 并谈一谈字符串和本地化。

若要启用的呈现表示层，可以利用我们的下 /src/resources/strings-strings.resjson 文件的所有字符串本地化它已设置。 当需要将一个新字符串添加到你的扩展时，将其添加到此 resjson 文件作为新项。 现有结构遵循以下格式：

``` ts
"<YourExtensionName>_<Component>_<Accessor>": "Your string value goes here.",
```

可以使用您喜欢的任何格式的字符串，但请注意生成过程 （resjson，然后输出可用的 TypeScript 类的进程） 将下划线 (_) 转换为句点 （.）。

例如，此项：
``` ts
"HelloWorld_cim_title": "CIM Component",
```
生成以下访问器结构：
``` ts
MsftSme.resourcesStrings<Strings>().HelloWorld.cim.title;
```
