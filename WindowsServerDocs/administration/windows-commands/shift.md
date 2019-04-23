---
title: shift
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 52b3012a39409f9d48ae8aa7608e7bd0af5787d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858818"
---
# <a name="shift"></a>shift



更改的批处理参数中的批处理文件的位置。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
shift [/n <N>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/n \<N>|指定在处开始更改*N*个自变量，其中*N*是从 0 到 8 的任何值。 需要命令扩展，默认情况下启用。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   **Shift**命令将更改批参数的值 **%0**通过 **%9**通过将每个参数复制到上一个-的值 **%1**复制到 **%0**的值 **%2**复制到 **%1**，依次类推。 这是用于编写执行任意数量的参数上的相同操作的批处理文件。
-   如果启用了命令扩展， **shift**命令支持 **/n**命令行选项。 **/N**选项指定开始更改在第 n 个参数，其中**N**是从 0 到 8 的任何值。 例如， **SHIFT/2**可能会改变 **%3**到 **%2**， **%4**到 **%3**，依此类推，并将 **%0**并 **%1**不受影响。 默认情况下启用命令扩展。
-   可以使用**shift**命令以创建可接受 10 个以上的批参数的批处理文件。 如果在命令行上指定 10 个以上的参数，则这些的显示后第 10 个 (**%9**) 将是到一次移动的一个 **%9**。
-   **Shift**命令不起任何作用**% \*** 批处理参数。
-   没有不向后**shift**命令。 在实现后**shift**命令时，无法恢复批参数 (**%0**) 的班次之前已存在。

## <a name="BKMK_examples"></a>示例

名为 Mycopy.bat 的示例批处理文件中的以下行演示如何使用**shift**与任意数量的批参数。 在此示例中，Mycopy.bat 将复制到特定目录的文件的列表。 按目录和文件名称参数表示批参数。
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

[命令行语法解答](command-line-syntax-key.md)