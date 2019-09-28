---
title: 面向不同版本的 Windows 管理中心 SDK
description: 面向不同版本的 Windows 管理中心 SDK （Project Honolulu）
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 0d3b7af5229f7b8487aa9f04eaf0d1756d8c02f4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356966"
---
# <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>面向不同版本的 Windows 管理中心 SDK

>适用于：Windows Admin Center、Windows Admin Center 预览版

通过 SDK 更改和平台更改，使扩展保持最新状态非常简单。  我们使用[NPM 标记](https://www.npmjs.com/package/@microsoft/windows-admin-center-sdk)将新功能的发布组织到 SDK 版本中。

有三个可供选择的 SDK 版本：

* ```latest``` –此 SDK 包与 Windows 管理中心中心的当前 GA 版本一致
* ```insider``` –此 SDK 包与 Windows 管理中心中心的当前预览版本（可在 Windows Server 有问必答 Preview 中提供）配合使用
* ```next``` –此 SDK 包包含最新功能

> [!NOTE]
> 详细了解可供下载的 Windows 管理中心的不同[版本](https://aka.ms/WACDownloadPage)。

## <a name="targeting-sdk-version-on-a-new-project"></a>面向新项目的 SDK 版本

创建新扩展时，可以包含 ```--version``` 参数以面向不同版本的 SDK：

```
wac create --company "{!Company Name}" --tool "{!Tool Name}" --version {!version}
```

| ReplTest1 | 说明 | 示例 |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | 公司名称（包含空格） | ```Contoso Inc``` |
| ```{!Tool Name}``` | 工具名称（包含空格） | ```Manage Foo Works``` |
| ```{!version}``` | SDK 版本 | ```latest``` |

下面是一个示例，用于创建新的扩展目标 ```insider```：

```
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version insider
```

## <a name="targeting-sdk-version-on-an-existing-project"></a>针对现有项目的 SDK 版本

若要修改现有项目以使其面向不同的 SDK 版本，请在 ```package.json``` 中修改以下行：

```
"@microsoft/windows-admin-center-sdk": "latest",
```
在此示例中，将 ```latest``` 替换为所需的 SDK 版本，即 ```insider```：

```
"@microsoft/windows-admin-center-sdk": "insider",
```

然后，运行 ```npm install``` 以便更新整个项目中的引用。