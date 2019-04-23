---
title: 面向不同版本的 Windows Admin Center SDK
description: 面向不同版本的 Windows Admin Center SDK (项目 Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 47ae669e517f963762ee6267594e18f3413a72ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833618"
---
# <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>面向不同版本的 Windows Admin Center SDK

>适用于：Windows Admin Center，Windows Admin Center 预览版

保持您的扩展插件的最新 SDK 更改和平台的更改非常简单。  我们使用[NPM 标记](https://www.npmjs.com/package/@microsoft/windows-admin-center-sdk)将组织的新功能发布到 SDK 版本。

有三个您可以从选择的 SDK 版本：

* ```latest``` -此 SDK 包与 Windows Admin Center 最新 GA 版本保持一致
* ```insider``` -此 SDK 包符合当前的预览版本的 Windows Admin Center （可从 Windows Server Insider Preview）
* ```next``` -此 SDK 包中包含的最新功能

> [!NOTE]
> 详细了解不同[版本](https://aka.ms/WACDownloadPage)Windows Admin Center 可供下载。

## <a name="targeting-sdk-version-on-a-new-project"></a>一个新项目的目标 SDK 版本

在创建一个新的扩展，可以包括```--version```参数，以面向不同版本的 sdk:

```
wac create --company "{!Company Name}" --tool "{!Tool Name}" --version {!version}
```

| ReplTest1 | 说明 | 示例 |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | 你的公司名称 （包含空格） | ```Contoso Inc``` |
| ```{!Tool Name}``` | 你的工具名称 （包含空格） | ```Manage Foo Works``` |
| ```{!version}``` | SDK 版本 | ```latest``` |

下面是一个示例创建新的扩展面向```insider```:

```
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version insider
```

## <a name="targeting-sdk-version-on-an-existing-project"></a>现有项目的目标 SDK 版本

若要修改现有项目以面向不同的 SDK 版本，修改以下行中的```package.json```:

```
"@microsoft/windows-admin-center-sdk": "latest",
```
在此示例中，替换```latest```所需的 SDK 版本，即```insider```:

```
"@microsoft/windows-admin-center-sdk": "insider",
```

然后运行```npm install```更新整个项目的引用。