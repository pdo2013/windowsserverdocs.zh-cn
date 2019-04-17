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
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/22/2018
ms.locfileid: "2081869"
---
# <a name="metadata-and-markdown-template"></a>元数据和减价模板

此操作模板包含示例减价语法，以及设置元数据的指南。 若要充分利用它，您必须查看[原始减价](https://raw.githubusercontent.com/Microsoft/WindowsServerDocs-pr/master/Contributor-guide/ws-template.md?token=AG1vEhARRHNLtPgKXP35BGjNZGajKOArks5YLNIwwA%3D%3D)和[呈现视图](https://github.com/Microsoft/WindowsServerDocs-pr/blob/master/Contributor-guide/ws-template.md)。 （原始减价显示元数据块中，而呈现的视图不。）

创建减价文件时，应将此模板复制到新文件、 填写元数据，下面指定的设置为的文章，标题上方的 H1 标题和删除内容。 需要提醒您注意 CAPS 在方括号中的任何内容。


## <a name="metadata"></a>Metadata 

上面的完整的元数据块。 一些重要注意事项：

- 您**必须**具有一个冒号 （:） 和元数据元素的值之间的空格。
- 值 （例如，标题） 中的冒号中断元数据分析程序。 在其位置中，使用的一个冒号的 HTML 编码`&#58;`(例如， `"title: Azure Rights Management&#58; the basics | Azure RMS"`)。
- **标题**： 此标题将出现在搜索引擎的结果。 
- **作者**: 作者域应包含作者，而不是其别名**GitHub 用户名**。
- **ms.prod**、 **ms.technology**： 用于 ms.prod （或 w10 如果您使用此模板来创建内容的 Windows 10） 的"windows 服务器阈值"。 与 CX 联系人获取 ms.technology 值。

## <a name="basic-markdown-gfm-and-special-characters"></a>基本减价、 GFM 和特殊字符

支持所有基本和 GitHub flavored 减价。 有关这些的详细信息，请参阅：

- [比较基准减价语法](https://daringfireball.net/projects/markdown/syntax)
- [GitHub flavored 减价 (GFM) 文档](https://guides.github.com/features/mastering-markdown)

减价使用特殊字符，如 \ *，\，并 \ # 的格式。 如果您希望在您的内容中包含下列字符之一，您必须执行以下两项之一：

- Put 特殊字符""对其进行转义前的反斜杠 (例如，\\\ * 为 \ *)
- [HTML 实体代码](http://www.ascii.cl/htmlcodes.htm)用于字符 (例如，\ & \#42\; & #42;)。

## <a name="headings"></a>标题

应完成标题 atx 样式，即使用，1-6 哈希字符 （#） 行的开头以指示的标题，通过 H6 HTML 标题级别 H1 相对应。 第一个和第二层标头的示例使用上方。 

存在**必须只有一个第一级标题 (H1)，您将显示为页面上标题的主题**。  

第二层标题将生成"这篇文章"部分中的页上标题下方显示的页上目录。

### <a name="third-level-heading"></a>第三级标题
#### <a name="fourth-level-heading"></a>第四级标题
##### <a name="fifth-level-heading"></a>第五个级别的标题
###### <a name="sixth-level-heading"></a>第六个一级标题

## <a name="text-styling"></a>文本样式

*斜体* 

**加粗** 

~~删除线~~

## <a name="links"></a>Links

### <a name="internal-links"></a>内部链接

要链接到同一个减价文件中的标头，查看已发布的文章的源、 查找标头的 ID (例如， `id="blockquote"`)，并使用 # + id 链接 (例如， `#blockquote`)。

- 示例： [Blockquotes](#blockquote)

要链接到在同一 repo 减价文件，使用[相对链接](https://www.w3.org/TR/WD-html40-970917/htmlweb.html#h-5.1.2)，包括文件名的末尾的".md"。

- 示例：[提示和问题](tips-gotchas.md)
- 示例：[工具和安装程序参与者 （英文）](../readme.md)

若要链接到在同一 repo 减价文件中的标头，请使用相对链接 + 井号链接。

- 示例：[删除文件](tips-gotchas.md#deleting-files)

### <a name="external-links"></a>外部链接

要链接到外部文件，请使用完整的 URL 作为的链接。

- 示例： [GitHub](http://www.github.com)

如果减价文件中显示的 URL，它将转换为可单击的链接。

- 示例：http://www.github.com

## <a name="lists"></a>列表

### <a name="ordered-lists"></a>已排序的列表

1. 此  
1. 是
1. 一个
1. 排序
1. 列表  


#### <a name="ordered-list-with-an-embedded-list"></a>按顺序与嵌入列表的列表

1. 此处
1. 方面
1. 一个
1. 嵌入
    1. 未命中王兰
    1. 教授梅
1. 排序
1. 列表


### <a name="unordered-lists"></a>无序的列表

- 此 
- 为
- a
- 项目符号
- 列表


##### <a name="unordered-list-with-an-embedded-list"></a>无序与嵌入列表的列表

- 此  
- 项目符号 
- 列表
    - 小姐孔雀开屏
    - 尊敬绿色
- 包含  
- 其他
    1. Colonel Mustard
    1. 小姐白皮书
- 列表


## <a name="horizontal-rule"></a>水平标尺

---

## <a name="tables"></a>表

在几乎每个实例中，使用 MD 表格格式。 虽然 HTML 表格提供更大的灵活性我们不可以在我们的内容中使用它们。 如果已在您的文章中的 HTML 表，我们不会合并的文章。

| 表        | 是           | 冷却  |
| ------------- |:-------------:| -----:|
| col 3 是      | 右对齐 | $ 1600 |
| col 2 是      | 居中      |   12 美元 |
| col 1 是默认按钮 | 左对齐     |    $ 1 |


## <a name="code"></a>代码

### <a name="generic-codeblock"></a>泛型 codeblock

缩进泛型 codeblock 编码代码四倍。

    function fancyAlert(arg) {
      if(arg) {
        $.docs({div:'#foo'})
      }
    }


### <a name="codeblocks-with-language-identifier"></a>Codeblocks 与语言标识符

使用三个 backticks (& #96; #96; & #96;) + 语言 ID 应用特定语言的颜色编码的代码块。  下面是[GitHub Flavored 减价 (GFM) 语言 Id](https://github.com/jmm/gfm-lang-ids/wiki/GitHub-Flavored-Markdown-(GFM)-language-IDs)的完整列表。

##### <a name="c9839"></a>C = #9839;

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

### <a name="inline-code"></a>内嵌代码

使用 backticks (& #96;) 的`inline code`。

## <a name="blockquotes"></a>Blockquotes

> Drought 具有万个十年来，现在持续和的严重蜥蜴年号长此后已经结束。 此处赤道，在其中将作为非洲、 已知一天洲上成功了具有达到 ferocity，新 climax，victor 尚未未始终可见。 在此 barren 和 desiccated 园地，仅小型或 swift 或激烈无法花边，或者甚至希望忞眦。

## <a name="images"></a>Images

### <a name="static-image"></a>静态图像

![这是替换文字](../windowsserverdocs/get-started/media/wsbanner.png)

### <a name="linked-image"></a>链接的图像

[![a链接的图像的 lt 文本](../windowsserverdocs/get-started/nano.png)](../windowsserverdocs/get-started/getting-started-with-nano-server.md) 

## <a name="alerts"></a>警报

### <a name="note"></a>注意

> [!NOTE]
> 这是注释

### <a name="warning"></a>警告

> [!WARNING]
> 这警告

### <a name="tip"></a>提示

> [!TIP]
> 这是提示

### <a name="important"></a>重要

> [!IMPORTANT]
> 这是重要说明

