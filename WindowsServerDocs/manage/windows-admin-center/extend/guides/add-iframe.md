---
title: 将 iFrame 添加到工具扩展中
description: 开发工具扩展 Windows 管理中心 SDK （Project Honolulu）-将 iFrame 添加到工具扩展
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 0833b2fd92f2bf4b512120783bb71295a3112745
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406891"
---
# <a name="add-an-iframe-to-a-tool-extension"></a>将 iFrame 添加到工具扩展中

>适用于：Windows Admin Center、Windows Admin Center 预览版

本文介绍如何将 iFrame 添加到使用 Windows 管理中心 CLI 创建的新的空工具扩展。

## <a name="prepare-your-environment"></a>准备你的环境 ##

如果尚未这样做，请按照[开发工具扩展](../develop-tool.md)中的说明来准备环境，并创建新的空工具扩展。

## <a name="add-a-module-to-your-project"></a>向项目添加模块 ##

向你的项目添加一个新的[空模块](add-module.md)，我们将在下一步中添加 iFrame。  

## <a name="add-an-iframe-to-your-module"></a>将 iFrame 添加到模块 ##

现在，我们将为刚创建的新的空模块添加 iFrame。

在 \src\app @ no__t-0 中，浏览到模块文件夹，然后打开具有以下命名约定的文件 ```{!module-name}.component.html```：

| ReplTest1 | 说明 | 示例文件名 |
| ----- | ----------- | ------- |
| ```{!module-name}``` | 模块名称（小写，空格替换为短划线） | ```manage-foo-works-portal.component.html``` |
    
将以下内容添加到 html 文件：

``` html
<div>
  <iframe  style="height: 850px;" src="https://www.bing.com"></iframe>
</div>
```

就是这样，你已将 iFrame 添加到了你的扩展。  接下来，你可以在 Windows 管理中心中[构建和端加载](../develop-tool.md#build-and-side-load-your-extension)扩展，以查看结果。

> [!Note]
> 内容安全策略（CSP）设置可能会阻止某些站点在 Windows 管理中心内的 iFrame 中呈现。 可在[此处](https://content-security-policy.com/)了解有关此内容的详细信息。 
