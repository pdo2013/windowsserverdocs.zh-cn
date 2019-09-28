---
title: PowerShell 模块创作注意事项
description: PowerShell 模块创作注意事项
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: JasonSh
author: lzybkr
ms.date: 10/16/2017
ms.openlocfilehash: 8945339e7a7950d3cd722ab2af629b45e7f6dd5d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370357"
---
# <a name="powershell-module-authoring-considerations"></a>PowerShell 模块创作注意事项

本文档包括一些与模块的创作方式相关的准则，以获得最佳性能。

## <a name="module-manifest-authoring"></a>模块清单创作

不使用以下准则的模块清单可能会对常规 PowerShell 性能产生显著影响，即使该模块未在会话中使用也是如此。

命令自动发现会分析每个模块，以确定模块导出的命令以及此分析的开销。
模块分析的结果按用户进行缓存，但在首次运行时，不能使用缓存，这是容器的典型方案。
在模块分析过程中，如果可以从清单中完全确定导出的命令，则可以避免更昂贵的模块分析。

### <a name="guidelines"></a>指南

* 在模块清单中，不要在 `AliasesToExport`、`CmdletsToExport` 和 `FunctionsToExport` 条目中使用通配符。

* 如果该模块未导出特定类型的命令，则通过指定 `@()` 在清单中显式指定此项。
缺少或 `$null` 条目等效于指定通配符 `*`。

应尽量避免以下情况：

```PowerShell
@{
    FunctionsToExport = '*'

    # Also avoid omitting an entry, it is equivalent to using a wildcard
    # CmdletsToExport = '*'
    # AliasesToExport = '*'
}
```

而应使用：

```PowerShell
@{
    FunctionsToExport = 'Format-Hex', 'Format-Octal'
    CmdletsToExport = @()  # Specify an empty array, not $null
    AliasesToExport = @()  # Also ensure all three entries are present
}
```

## <a name="avoid-cdxml"></a>避免 CDXML

决定如何实现模块时，有三种主要选择：

* 二进制（通常C#为）
* 脚本（PowerShell）
* CDXML （XML 文件包装 CIM）

如果加载模块的速度非常重要，则 CDXML 的速度大致低于二进制模块。

二进制模块的加载速度最快，因为它是提前编译的，并且可以使用 NGen 对每台计算机进行 JIT 编译。

脚本模块的加载速度通常比二进制模块更慢，因为在编译和执行该脚本之前，PowerShell 必须对其进行分析。

CDXML 模块的速度通常比脚本模块慢很多，因为它必须首先分析 XML 文件，该文件随后会生成相当多的 PowerShell 脚本，然后进行分析和编译。

