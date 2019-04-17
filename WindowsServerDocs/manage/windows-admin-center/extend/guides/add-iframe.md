---
title: 将 iFrame 添加到工具扩展
description: 开发工具扩展 Windows Admin Center SDK (Project Honolulu)-将 iFrame 添加到工具扩展
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 7b4a7b688e4b2d9f52e44395c19211b91b964578
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/20/2018
ms.locfileid: "4080924"
---
# 将 iFrame 添加到工具扩展

>适用于：Windows Admin Center、Windows Admin Center 预览版

在本文中，我们将为我们创建了使用 Windows Admin Center CLI 的新的空工具扩展添加 iFrame。

## 准备你的环境 ##

如果你尚未这样做，请按照[开发工具扩展](..\develop-tool.md)以准备环境，并创建新的空工具扩展。

## 向项目添加一个模块 ##

添加到你的项目，向其我们将在下一步中添加 iFrame 的一个新的[空模块](add-module.md)。  

## 将 iFrame 添加到你的模块 ##

现在，我们将添加到刚创建的新的空模块的 iFrame。

在 \src\app\，浏览到你的模块文件夹，然后打开文件```{!module-name}.component.html```使用以下命名约定找到：

| 值 | 说明 | 示例文件名 |
| ----- | ----------- | ------- |
| ```{!module-name}``` | 模块名称（小写，空格替换为短划线） | ```manage-foo-works-portal.component.html``` |
    
向 html 文件中添加以下内容：

``` html
<div>
  <iframe  style="height: 850px;" src="https://www.bing.com"></iframe>
</div>
```

以上就是，你添加到你的扩展 iFrame。  接下来，你可以[构建并旁加载](..\develop-tool.md#build-and-side-load-your-extension)你的扩展 Windows Admin Center，以查看结果中。
