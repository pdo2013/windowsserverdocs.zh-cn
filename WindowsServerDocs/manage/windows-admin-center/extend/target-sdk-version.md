---
title: 目标是不同版本的 Windows Admin Center SDK
description: 目标是不同版本的 Windows Admin Center SDK (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 47ae669e517f963762ee6267594e18f3413a72ff
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/20/2018
ms.locfileid: "4081034"
---
# 目标是不同版本的 Windows Admin Center SDK

>适用于：Windows Admin Center、Windows Admin Center 预览版

使你的扩展 SDK 更改和平台的更改保持最新很容易。  我们使用[NPM 标记](https://www.npmjs.com/package/@microsoft/windows-admin-center-sdk)组织成 SDK 版本的新功能发布。

有三个 SDK 版本，你可以选择从：

* ```latest``` – 此 SDK 包与当前的 GA 版本的 Windows Admin Center 对齐
* ```insider``` – 此 SDK 包的当前预览版本的 Windows Admin Center （可在 Windows Server Insider Preview） 与一致
* ```next``` – 此 SDK 包包含最新功能

> [!NOTE]
> 了解有关不同[版本](https://aka.ms/WACDownloadPage)的 Windows Admin Center 可供下载的详细信息。

## 在新项目的目标 SDK 版本

在创建新的扩展时，你可以包含```--version```参数，以针对不同版本的 SDK:

```
wac create --company "{!Company Name}" --tool "{!Tool Name}" --version {!version}
```

| 值 | 说明 | 示例 |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | 你的公司名称 （使用空格） | ```Contoso Inc``` |
| ```{!Tool Name}``` | 你的工具名称 （使用空格） | ```Manage Foo Works``` |
| ```{!version}``` | SDK 版本 | ```latest``` |

下面是示例创建新的扩展面向```insider```:

```
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version insider
```

## 在现有项目的目标 SDK 版本

若要修改现有项目以不同的 SDK 版本为目标，请修改中的以下行```package.json```:

```
"@microsoft/windows-admin-center-sdk": "latest",
```
在此示例中，替换```latest```与你所需的 SDK 版本，即```insider```:

```
"@microsoft/windows-admin-center-sdk": "insider",
```

然后运行```npm install```更新整个项目的引用。