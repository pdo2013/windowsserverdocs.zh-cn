---
title: PowerShell 脚本的性能注意事项
description: 有关性能的 PowerShell 脚本
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: JasonSh
author: lzybkr
ms.date: 10/16/2017
ms.openlocfilehash: df406c7e382907ca32006ec4dae6537a140a91cf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830368"
---
# <a name="powershell-scripting-performance-considerations"></a>PowerShell 脚本的性能注意事项

PowerShell 脚本用于直接利用.NET 和避免管道通常比惯用 PowerShell 更快。 惯用 PowerShell 通常使用 cmdlet 和 PowerShell 函数很大程度，经常利用管道，并放入.NET 仅在必要时向下。

>[!Note] 
> 许多此处所述的技术不是惯用的 PowerShell，可能会降低 PowerShell 脚本的可读性。 建议除非性能另外规定，否则使用惯用的 PowerShell 脚本创建者。

## <a name="suppressing-output"></a>禁止显示输出

有许多种方法可以避免对象写入管道：

```PowerShell
$null = $arrayList.Add($item)
[void]$arrayList.Add($item)
```

分配给`$null`等强制转换到或`[void]`是大致等效的通常应首选的性能很重要。

```PowerShell
$arrayList.Add($item) > $null
```

文件重定向到`$null`几乎就是上一个替代方法，最脚本将永远不会注意到差异。
根据方案，文件重定向会但引入很少的开销。

```PowerShell
$arrayList.Add($item) | Out-Null
```

通过管道传递到`Out-Null`具有很大的开销相比备选方法时。
它应避免在性能敏感代码中。

```PowerShell
$null = . {
    $arrayList.Add($item)
    $arrayList.Add(42)
}
```

引入脚本块，并调用它 （使用圆点来源或其他），然后将分配到结果`$null`便利的技术，用于取消一个大的脚本块的输出。
此技术以及对管道执行大致`Out-Null`，应当避免性能敏感的脚本中。
在此示例中的额外系统开销来自于创建和调用以前是内联脚本的脚本块。


## <a name="array-addition"></a>数组添加

生成的项的列表通常是一个数组中使用加法运算符：

```PowerShell
$results = @()
$results += Do-Something
$results += Do-SomethingElse
$results
```

这可能会非常 inefficent，因为数组是固定不变。
数组的每个添加实际创建一个新数组足够大以容纳两个左和右操作数中的所有元素，然后将两个操作数的元素复制到新数组。
对于小集合，这种开销也没有关系。
对于大型集合，这可以肯定是问题。

有几个备选方法。
如果实际上不需要执行一个数组，而是请考虑使用 ArrayList:

```PowerShell
$results = [System.Collections.ArrayList]::new()
$results.AddRange((Do-Something))
$results.AddRange((Do-SomethingElse))
$results
```

如果您需要一个数组，则可以使用你自己`ArrayList`，只需调用`ArrayList.ToArray`时所需的数组。
或者，可以让创建的 PowerShell`ArrayList`和`Array`为您：

```PowerShell
$results = @(
    Do-Something
    Do-SomethingElse
)
```

在此示例中，PowerShell 将创建`ArrayList`来存储结果写入到数组表达式中的管道。
之前将分配给`$results`，PowerShell 会将转换`ArrayList`到`object[]`。

## <a name="processing-large-files"></a>处理大型文件

处理文件，在 PowerShell 中的惯用方法可能如下所示：

```PowerShell
Get-Content $path | Where-Object { $_.Length -gt 10 }
```

这可以是几乎一个数量级慢于直接使用.NET Api:

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

它通常被认为更好的做法将输出写入到控制台中，直接但是时非常有用，许多脚本都使用`Write-Host`。

如果必须将多条消息写入到控制台中，`Write-Host`可能会慢于一个数量级`[Console]::WriteLine()`。 但是，请注意，`[Console]::WriteLine()`是仅适用的替代方法的特定主机，例如 powershell.exe 或 powershell_ise.exe-不保证它已在所有主机中工作。

而不是使用`Write-Host`，请考虑使用[Write-output](/powershell/module/Microsoft.PowerShell.Utility/Write-Output?view=powershell-5.1)。

