---
title: shift
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b56574e8-570a-4cc9-bbac-1b94fbf6a47a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f74e0f1f9041a4a7b95d83772ea79376c82876de
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371249"
---
# <a name="shift"></a>shift



更改批处理文件中批处理参数的位置。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
shift [/n <N>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/n \<N >|指定从第*n*个参数开始移位，其中*n*是从0到8的任何值。 需要命令扩展，默认情况下已启用。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

- **移位**命令通过将每个参数复制到前一个参数来更改批参数 **% 0**到 **% 9**的值，将 **% 1**的值复制到 **% 0**，将 **% 2**的值复制到 **% 1**，依此类推。 这适用于写入对任意数量的参数执行相同操作的批处理文件。
- 如果启用了命令扩展，则**shift**命令支持 **/n**命令行选项。 **/N**选项指定在第 n 个参数处开始移位，其中**n**是从0到8的任何值。 例如， **shift/2**会将 **% 3**移位到% **2**， **% 4**转换为 **% 3**，依此类推，并不影响 **% 0**和 **% 1** 。 默认情况下启用命令扩展。
- 你可以使用**shift**命令创建一个可接受超过10个批处理参数的批处理文件。 如果在命令行中指定了10个以上的参数，则在第十个（ **% 9**）后出现的参数将一次移位到 **% 9**中。
- **Shift**命令对 **% @ no__t*** batch 参数无效。
- 没有**后退命令**。 实现**shift**命令后，将无法恢复在移位之前存在的批处理参数（ **% 0**）。

## <a name="BKMK_examples"></a>示例

名为 Mycopy 的示例批处理文件中的以下行演示了如何对任意数量的批处理参数使用**shift** 。 在此示例中，Mycopy 将文件列表复制到特定目录。 批处理参数由目录和文件名参数表示。
```
@echo off 
rem MYCOPY.BAT copies any number of files
rem to a directory.
rem The command uses the following syntax:
rem mycopy dir file1 file2 ... 
set todir=%1
:getfile
shift
if "%1"=="" goto end
copy %1 %todir%
goto getfile
:end
set todir=
echo All done
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)