---
title: start
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0173f9b3-5cd7-4edb-b01e-d02193b4fadc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 59927015f63243f16ba6e9674bc74adbd3c4f96a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852628"
---
# <a name="start"></a>start



将启动单独的命令提示符窗口以运行指定的程序或命令。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
start ["<Title>"] [/d <Path>] [/i] [{/min | /max}] [{/separate | /shared}] [{/low | /normal | /high | /realtime | /abovenormal | belownormal}] [/affinity <HexAffinity>] [/wait] [/b {<Command> | <Program>} [<Parameters>]]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|"\<标题 >"|指定要在命令提示符窗口标题栏中显示的标题。|
|/d \<Path>|指定的启动目录。|
|i|将 Cmd.exe 启动环境传递给新的命令提示符窗口。 如果 **/i**未指定，则使用当前的环境。|
|/min  \| /最大值|指定最大限度地 (**/min**) 或最大化 (**/最大**) 新的命令提示符窗口。|
|/separate \| /shared|启动单独的内存空间中的 16 位程序 (**/separate**) 或共享内存空间 (**/shared**)。 在 64 位平台上不支持这些选项。|
|/ 低\|/正常\|/高\|/realtime \| /abovenormal \| /belownormal|在指定的优先级类中启动应用程序。 有效的优先级类的值为 **/低**， **/正常**， **/高**， **/realtime**， **/abovenormal**，并 **/belownormal**。|
|/affinity \<HexAffinity >|适用于新的应用程序指定的处理器关联掩码 （以十六进制数表示）。|
|/wait|启动应用程序，等待它结束。|
|/b|而无需打开新的命令提示符窗口中启动应用程序。 CTRL + C 处理被忽略，除非应用程序启用 CTRL + C 处理。 使用 CTRL + BREAK 中断应用程序。|
|/b\<命令 > \| \<程序 >|指定要启动的命令或程序。|
|\<参数 >|指定要传递到命令或程序的参数。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   通过键入文件的名称作为命令，可以通过其文件关联运行该文件。
-   当没有扩展名或路径的限定符的第一个标记为运行命令，其中包含字符串"CMD"时，"CMD"替换 COMSPEC 变量的值。 这可以防止用户选取**cmd**当前目录中。
-   当运行 32 位图形用户界面 (GUI) 应用程序， **cmd**不会等待应用程序返回到命令提示符前退出。 如果从命令脚本运行该应用程序，此行为不会发生。
-   运行时使用的第一个令牌不包含扩展的命令，Cmd.exe 使用 PATHEXT 环境变量的值来确定哪些扩展，以查找和以何种顺序。 PATHEXT 变量的默认值是：  
    ```
    .COM;.EXE;.BAT;.CMD 
    ```  
    请注意，语法是 PATH 变量，用分号分隔每个扩展相同。
-   如果没有匹配项的扩展名，有关可执行文件，搜索时**启动**检查名称是否与目录名称匹配。 如果是这样，**启动**在该路径上打开 Explorer.exe。

## <a name="BKMK_examples"></a>示例

若要启动的命令提示符处的 Myapp 程序并保持使用当前的命令提示符窗口，键入：
```
start myapp 
```
若要查看**启动**在单独的命令行帮助主题最大化命令提示符窗口中，键入：
```
start /max start /?
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
