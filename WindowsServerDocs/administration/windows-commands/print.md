---
title: print
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aa2325d5-a993-4ed3-b996-255165452db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d85fc5b2cd5f5ba09ebdf4756a5adb60c1759f2a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831548"
---
# <a name="print"></a>print



将文本文件发送到打印机。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
Print [/d:<PrinterName>] [<Drive>:][<Path>]<FileName>[ ...]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/d:\<PrinterName >|指定你想要打印作业的打印机。 若要打印到本地连接打印机，请将打印机连接，在计算机上指定的端口。</br>-并行端口有效的值为 LPT1、 LPT2 和 LPT3。</br>-有效的串行端口的值为 COM1、 COM2、 COM3 和 COM4。</br>此外可以使用队列名称来指定网络打印机 (\\\\*ServerName*\*PrinterName *)。 如果未指定打印机，打印作业发送到 LPT1 默认情况下。|
|\<驱动器 >:|指定要打印的文件的所在位置的逻辑或物理驱动器。 如果您想要打印的文件位于当前驱动器上，则不需要此参数。|
|\<Path>|指定要打印的文件的位置。 如果您想要打印的文件位于当前目录中，则不需要此参数。|
|\<FileName>[ ...]|必需。 指定要打印的文件。 可以在一个命令中包含多个文件。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   如果将其发送到打印机连接到本地计算机上的串行或并行端口，可以在后台打印文件。
-   可以通过在命令提示符下执行许多配置任务**模式下**命令。

    请参阅[模式下](mode.md)有关详细信息：  
    -   配置打印机连接到并行端口
    -   配置打印机连接到串行端口
    -   显示打印机的状态
    -   为代码页切换准备打印机

## <a name="BKMK_examples"></a>示例

若要发送到打印机的当前目录中的 Report.txt 连接 LPT2 到本地计算机的文件，请键入：
```
print /d:lpt2 report.txt
```
将发送到 Printer1 打印队列的 c:\Accounting 目录中的文件 Report.txt \\ \\CopyRoom 服务器，键入：
```
print /d:\\copyroom\printer1 c:\accounting\report.txt 
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

[打印命令参考](print-command-reference.md)

[模式](mode.md)