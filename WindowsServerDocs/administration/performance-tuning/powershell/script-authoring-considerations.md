---
title: PowerShell 脚本性能注意事项
description: 在 PowerShell 中编写性能脚本
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: JasonSh
author: lzybkr
ms.date: 10/16/2017
ms.openlocfilehash: 2898cf5ee965da77c9f6a3473e55c1cee6b53f2b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71354976"
---
# <a name="powershell-scripting-performance-considerations"></a>PowerShell 脚本性能注意事项

直接利用 .NET 并避免管道的 PowerShell 脚本的速度要快于惯用 PowerShell。 惯用 PowerShell 通常使用 cmdlet 和 PowerShell 函数，通常利用管道，并且仅在必要时才会将其放入 .NET。

>[!Note] 
> 此处所述的许多方法并不惯用 PowerShell，可能降低 PowerShell 脚本的可读性。 建议编写作者使用惯用 PowerShell，除非性能决定。

## <a name="suppressing-output"></a>禁止输出

有多种方法可避免将对象写入管道：

```PowerShell
$null = $arrayList.Add($item)
[void]$arrayList.Add($item)
```

分配到 @no__t 0 或强制转换到 @no__t 的操作大致等效，通常在性能方面很重要。

```PowerShell
$arrayList.Add($item) > $null
```

文件重定向到 `$null` 与以前的替代方法几乎一样，大多数脚本都不会注意到差异。
不过，文件重定向会稍微增加一些开销。

```PowerShell
$arrayList.Add($item) | Out-Null
```

与替代项相比，管道到 `Out-Null` 的开销很大。
它应避免对性能敏感的代码的影响。

```PowerShell
$null = . {
    $arrayList.Add($item)
    $arrayList.Add(42)
}
```

引入脚本块并调用它（使用网点或其他方式），然后将结果赋给 `$null` 是一种用于取消大型脚本块输出的便利方法。
此方法大致和管道执行 `Out-Null`，应避免在性能敏感的脚本中。
此示例中的额外开销来自于创建和调用以前内联脚本的脚本块。


## <a name="array-addition"></a>数组加法

通常使用带有加法运算符的数组来生成项列表：

```PowerShell
$results = @()
$results += Do-Something
$results += Do-SomethingElse
$results
```

由于数组是不可变的，因此这可能非常 inefficent。
对于数组的每个加法，实际上会创建一个足以容纳左操作数和右操作数的所有元素的新数组，然后将这两个操作数的元素复制到新数组中。
对于小型集合，这种开销可能并不重要。
对于大型集合，这可能是一个问题。

有一些替代方法。
如果实际上不需要数组，请考虑使用 ArrayList：

```PowerShell
$results = [System.Collections.ArrayList]::new()
$results.AddRange((Do-Something))
$results.AddRange((Do-SomethingElse))
$results
```

如果确实需要数组，则可以使用自己的 `ArrayList`，并在需要数组时直接调用 `ArrayList.ToArray`。
或者，你可以让 PowerShell 为你创建 `ArrayList` 并 `Array`：

```PowerShell
$results = @(
    Do-Something
    Do-SomethingElse
)
```

在此示例中，PowerShell 创建一个 `ArrayList` 来保存写入到数组表达式内的管道中的结果。
在分配到 `$results` 之前，PowerShell 将 @no__t 转换为 `object[]`。

## <a name="processing-large-files"></a>处理大型文件

在 PowerShell 中处理文件的惯用方法可能如下所示：

```PowerShell
Get-Content $path | Where-Object { $_.Length -gt 10 }
```

这可能比直接使用 .NET Api 时的数量级慢：

```PowerShell
try
{
    $stream = [System.IO.StreamReader]::new($path)
    while ($line = $stream.ReadLine())
    {
        if ($line.Length -gt 10)
        {
            $line
        }
    }
}
finally
{
    $stream.Dispose()
}
```

## <a name="avoid-write-host"></a>避免写入主机

通常情况下，将输出直接写入控制台通常是一种很好的做法，但当有意义时，很多脚本使用 `Write-Host`。

如果必须将多条消息写入控制台，`Write-Host` 的阶数可能比 `[Console]::WriteLine()` 慢。 但请注意，`[Console]::WriteLine()` 只是特定主机（如 powershell_ise 或）的一种合适的替代方法，这不能保证在所有主机中都能正常工作。

请考虑使用[写入输出](/powershell/module/Microsoft.PowerShell.Utility/Write-Output?view=powershell-5.1)，而不是使用 `Write-Host`。

