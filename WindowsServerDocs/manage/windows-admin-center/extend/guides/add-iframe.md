---
title: 将 iFrame 添加到工具扩展中
description: 开发工具扩展 Windows Admin Center SDK (项目 Honolulu)-将 iFrame 添加到工具扩展
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 7cf1dcec1bc8e187b6db789c5402ca8119ca8b6c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850758"
---
# <a name="add-an-iframe-to-a-tool-extension"></a>将 iFrame 添加到工具扩展中

>适用于：Windows Admin Center，Windows Admin Center 预览版

在本文中，我们将为空的新工具扩展，我们已创建了使用 Windows Admin Center CLI 添加 iFrame。

## <a name="prepare-your-environment"></a>准备你的环境 ##

如果尚未安装，请按照中的说明[开发的工具扩展](..\develop-tool.md)准备您的环境，创建了一个新的空工具扩展。

## <a name="add-a-module-to-your-project"></a>将模块添加到你的项目 ##

添加一个新[空模块](add-module.md)到项目中，到我们将在下一步中添加 iFrame。  

## <a name="add-an-iframe-to-your-module"></a>将 iFrame 添加到你的模块 ##

现在，我们会将 iFrame 添加到我们刚刚创建的该新的空模块。

在 \src\app\,浏览到模块文件夹，然后打开文件```{!module-name}.component.html```、 找到以下命名约定：

| ReplTest1 | 说明 | 示例文件名 |
| ----- | ----------- | ------- |
| ```{!module-name}``` | 模块名称（小写，空格替换为短划线） | ```manage-foo-works-portal.component.html``` |
    
将以下内容添加到 html 文件：

``` html
<div>
  <iframe  style="height: 850px;" src="https://www.bing.com"></iframe>
</div>
```

就这么简单，iFrame 添加到你的扩展。  接下来，你可以[生成并端负载](..\develop-tool.md#build-and-side-load-your-extension)将扩展 Windows Admin Center，以查看结果。

> [!Note]
> 内容安全策略 (CSP) 设置可能阻止 Windows Admin Center 中的 iFrame 中呈现的某些站点。 您可以了解有关这个[此处](https://content-security-policy.com/)。 
