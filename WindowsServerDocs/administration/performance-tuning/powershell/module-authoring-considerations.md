---
title: 创作注意事项的 PowerShell 模块
description: 创作注意事项的 PowerShell 模块
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: JasonSh
author: lzybkr
ms.date: 10/16/2017
ms.openlocfilehash: 37dd860019b91daf70947dba93d20274048487a0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818718"
---
# <a name="powershell-module-authoring-considerations"></a>创作注意事项的 PowerShell 模块

本文档包括与模块为获得最佳性能的编写方式相关的一些准则。

## <a name="module-manifest-authoring"></a>模块清单创作

不使用以下指导原则的模块清单可以具有常规 PowerShell 性能会受到明显影响，即使不在会话中使用的模块。

命令自动发现分析以确定哪些命令模块可导出和此分析可能很昂贵的每个模块。
模块分析的结果将被缓存每个用户，但在缓存不可用在首次运行，这是典型的方案中的容器。
模块在分析期间，如果导出的命令可以在清单中，完全确定开销更大的模块的分析，可以避免。

### <a name="guidelines"></a>指南

* 在模块清单，请使用中的通配符`AliasesToExport`， `CmdletsToExport`，和`FunctionsToExport`条目。

* 如果该模块不会导出特定类型的命令，此显式指定在清单中通过指定`@()`。
缺少或`$null`项是等效于指定通配符`*`。

应避免以下，在可能的情况：

```PowerShell
@{
    FunctionsToExport = '*'

    # Also avoid omitting an entry, it is equivalent to using a wildcard
    # CmdletsToExport = '*'
    # AliasesToExport = '*'
}
```

相反，使用：

```PowerShell
@{
    FunctionsToExport = 'Format-Hex', 'Format-Octal'
    CmdletsToExport = @()  # Specify an empty array, not $null
    AliasesToExport = @()  # Also ensure all three entries are present
}
```

## <a name="avoid-cdxml"></a>Avoid CDXML

在决定如何实现你的模块时，有三个主要选项：

* 二进制 (通常C#)
* 脚本 (PowerShell)
* CDXML （XML 文件包装 CIM）

如果加载模块的速度很重要，CDXML 大约是比二进制模块慢一个数量级。

二进制模块加载速度最快，因为它提前编译，并且可以使用一次每台计算机的 JIT 编译到 NGen。

脚本模块通常更慢于二进制模块加载，因为 PowerShell 必须分析之前编译并执行它，该脚本。

CDXML 模块是通常比脚本模块要慢得多，因为它必须首先分析会生成大量的 PowerShell 脚本，然后分析并编译的 XML 文件。

