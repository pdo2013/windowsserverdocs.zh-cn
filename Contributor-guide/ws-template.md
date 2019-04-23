---
title:
- 文章标题
description: ''
author:
- GITHUB USERNAME
ms.author:
- MICROSOFT ALIAS
ms.date:
- DATE
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: ''
ms.localizationpriority:
- high/medium/low
ms.openlocfilehash: 4f885680426c0bfa55d5f73a7ef0c2143a8dd5a9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879558"
---
# <a name="metadata-and-markdown-template"></a>元数据和 Markdown 模板

此操作模板包含 Markdown 语法，以及设置元数据的指南的示例。 若要充分利用它，您必须同时查看[原始 Markdown](https://raw.githubusercontent.com/Microsoft/WindowsServerDocs-pr/master/Contributor-guide/ws-template.md?token=AG1vEhARRHNLtPgKXP35BGjNZGajKOArks5YLNIwwA%3D%3D)并[呈现的视图](https://github.com/Microsoft/WindowsServerDocs-pr/blob/master/Contributor-guide/ws-template.md)。 （原始 Markdown 显示元数据块，而不呈现的视图。）

创建 Markdown 文件时，应将此模板复制到新文件中，填写元数据，以下指定的设置的文章，标题上方的 H1 标题并删除的内容。 用方括号括起来的 CAPS 中的任何内容需要关注。


## <a name="metadata"></a>元数据 

完整元数据块如上所示。 一些关键注意事项：

- 您**必须**有冒号 （:） 之间留一个空格和元数据元素的值。
- 值 (例如，title) 中的冒号会中断元数据解析器。 在其位置使用的冒号的 HTML 编码`&#58;`(例如， `"title: Azure Rights Management&#58; the basics | Azure RMS"`)。
- **标题**:此标题将显示在搜索引擎结果中。 
- **作者**:作者字段应包含**的 GitHub 用户名**的作者，而不是其别名。
- **ms.prod**， **ms.technology**:使用"windows server 阈值"ms.prod （或 w10 如果使用此模板适用于 Windows 10 中创建的内容）。 若要获取 ms.technology 值你 CX 联系人与进行通信。

## <a name="basic-markdown-gfm-and-special-characters"></a>基本 Markdown、 GFM 和特殊字符

支持所有基本和 GitHub 式 Markdown。 有关详细信息，请参阅：

- [基线 Markdown 语法](https://daringfireball.net/projects/markdown/syntax)
- [GitHub 式 Markdown (GFM) 文档](https://guides.github.com/features/mastering-markdown)

Markdown 使用特殊字符如\*， \`，和\#格式设置。 如果你想要在内容中包含这些字符之一，必须执行两项操作之一：

- 加上反斜杠之前的特殊字符来"转义"它 (例如， \\ \*为\*)
- 使用[HTML 实体代码](http://www.ascii.cl/htmlcodes.htm)的字符 (例如， \& \#42\;的&#42;)。

## <a name="headings"></a>标题

标题使用 atx 样式，也就是说，使用 1 个哈希字符 （#） 在行的开头来表示标题，对应于 HTML 标题级别 H1 到 H6。 上面使用的是第一个和第二个级别标头的示例。 

存在**必须**只有一个第一级别标题 (H1) 主题，其中将显示为页面上的标题中。  

第二级标题生成页标题下方的"在本文"部分中显示在页面内 TOC。

### <a name="third-level-heading"></a>第三级标题
#### <a name="fourth-level-heading"></a>第四级标题
##### <a name="fifth-level-heading"></a>第五级标题
###### <a name="sixth-level-heading"></a>六级标题

## <a name="text-styling"></a>文本样式

*斜体* 

**加粗** 

~~删除线~~

## <a name="links"></a>链接

### <a name="internal-links"></a>内部链接

若要链接到同一 Markdown 文件的标头，查看已发布项目的源，查找头 ID (例如， `id="blockquote"`)，并使用 # + id 进行链接 (例如， `#blockquote`)。

- 例如：[引用标记](#blockquote)

若要链接到同一个存储库中的 Markdown 文件，请使用[相对链接](https://www.w3.org/TR/WD-html40-970917/htmlweb.html#h-5.1.2)，包含在文件名末尾的".md"。

- 例如：[提示与陷阱](tips-gotchas.md)
- 例如：[工具和 contributors （参与者） 的安装程序](../readme.md)

若要链接到同一个存储库中的 Markdown 文件的标头，使用相对链接和井号标签链接。

- 例如：[正在删除文件](tips-gotchas.md#deleting-files)

### <a name="external-links"></a>外部链接

若要链接到外部文件，请使用为链接的完整 URL。

- 例如：[GitHub](http://www.github.com)

如果在 Markdown 文件中显示一个 URL，它将转换为可单击的链接。

- 示例： http://www.github.com

## <a name="lists"></a>列表

### <a name="ordered-lists"></a>排序的列表

1. 此 
1. 是
1. 一种是
1. 排序
1. 列表  


#### <a name="ordered-list-with-an-embedded-list"></a>经过排序的列表包含嵌套列表

1. 此处
1. 出现
1. 一个
1. 嵌入
    1. Scarlett 小姐
    1. Plum 教授
1. 排序
1. 列表


### <a name="unordered-lists"></a>未排序的列表

- 此
- 为
- a
- 项目符号
- 列表


##### <a name="unordered-list-with-an-embedded-list"></a>包含嵌套列表的未排序的列表

- 此 
- 项目符号 
- 列表
    - Peacock 夫人
    - Green 先生
- 包含  
- 其他
    1. Mustard 上校
    1. White 夫人
- 列出了


## <a name="horizontal-rule"></a>水平标尺

---

## <a name="tables"></a>表

在几乎每个实例中，使用 MD 表的格式设置。 HTML 表提供了更大的灵活性虽然我们不使用它们在我们的内容。 如果您的文章中有一个 HTML 表，我们不会合并该文章。

| 表        | 是           | 太棒了  |
| ------------- |:-------------:| -----:|
| 第 3 列是      | 右对齐 | $1600 |
| 第 2 列是      | 居中      |   12 美元 |
| 第 1 列是默认值 | left-aligned     |    $1 |


## <a name="code"></a>代码

### <a name="generic-codeblock"></a>泛型 codeblock

缩进泛型 codeblock 编写代码代码四个的空格。

    function fancyAlert(arg) {
      if(arg) {
        $.docs({div:'#foo'})
      }
    }


### <a name="codeblocks-with-language-identifier"></a>Codeblocks 具有语言标识符

使用三个反撇号 (&#96;&#96;&#96;) + 语言 ID，将应用特定于语言的颜色编码到代码块。  下面是整个列表[GitHub Flavored Markdown (GFM) 语言 Id](https://github.com/jmm/gfm-lang-ids/wiki/GitHub-Flavored-Markdown-(GFM)-language-IDs)。

##### <a name="c9839"></a>C&#9839;

```c#
using System;
namespace HelloWorld
{
    class Hello 
    {
        static void Main() 
        {
            Console.WriteLine("Hello World!");

            // Keep the console window open in debug mode.
            Console.WriteLine("Press any key to exit.");
            Console.ReadKey();
        }
    }
}
```
#### <a name="python"></a>Python

```python
friends = ['john', 'pat', 'gary', 'michael']
for i, name in enumerate(friends):
    print "iteration {iteration} is {name}".format(iteration=i, name=name)
```
#### <a name="powershell"></a>PowerShell

```powershell
Clear-Host
$Directory = "C:\Windows\"
$Files = Get-Childitem $Directory -recurse -Include *.log `
-ErrorAction SilentlyContinue
```

### <a name="inline-code"></a>内联代码

使用反撇号 (&#96;) 对于`inline code`。

## <a name="blockquotes"></a>引用标记

> 干旱持续一千万年和统治的可怕的蜥蜴早已已经结束。 此处在将一天是一种非洲，在大洲在赤道的大战的凶残已达到新的大陆，高潮和 victor 尚未建立直通连接。 在此对单调和干燥的土地仅的小或 swift 或敏捷无法壮大，或甚至希望生存。

## <a name="images"></a>映像

### <a name="static-image"></a>静态图像

![这是替换文字](../windowsserverdocs/get-started/media/wsbanner.png)

### <a name="linked-image"></a>链接的图像

[![链接图像的替换文字](../windowsserverdocs/get-started/nano.png)](../windowsserverdocs/get-started/getting-started-with-nano-server.md) 

## <a name="alerts"></a>警报

### <a name="note"></a>注意

> [!NOTE]
> 这是注意

### <a name="warning"></a>警告

> [!WARNING]
> 这警告

### <a name="tip"></a>提示

> [!TIP]
> 这是提示

### <a name="important"></a>重要

> [!IMPORTANT]
> 这是重要事项

