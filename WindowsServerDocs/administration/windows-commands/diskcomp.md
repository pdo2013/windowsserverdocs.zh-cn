---
title: diskcomp
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4f56f534-a356-4daa-8b4f-38e089341e42
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3945311873dff763a8e9e8cdd3f766c1ed4580b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837528"
---
# <a name="diskcomp"></a>diskcomp



比较两个软盘的内容。 如果使用不带参数， **diskcomp**使用当前的驱动器来比较这两个磁盘。有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
diskcomp [<Drive1>: [<Drive2>:]]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<Drive1>|指定包含一个软盘驱动器。|
|\<Drive2>|指定包含其他软盘驱动器。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   使用磁盘

    **Diskcomp**命令仅适用于软盘。 不能使用**diskcomp**的硬盘。 如果指定的硬盘驱动器*Drive1*或*驱动器 2*， **diskcomp**显示以下错误消息：  
    ```
    Invalid drive specification
    Specified drive does not exist
    or is nonremovable
    ```  
-   比较磁盘

    如果要比较的两个磁盘上的所有跟踪都都相同， **diskcomp**会显示以下消息：  
    ```
    Compare OK
    ```  
    如果跟踪不相同， **diskcomp**显示一条类似于以下消息：  
    ```
    Compare error on
    side 1, track 2
    ```  
    当**diskcomp**完成比较，则会显示以下消息：  
    ```
    Compare another diskette (Y/N)?
    ```  
    如果您按 Y **diskcomp**将提示您插入对磁盘进行下一步的比较。 如果按 N **diskcomp**停止比较。

    当**diskcomp**使比较，它会忽略磁盘的卷编号。
-   省略驱动器参数

    如果省略*驱动器 2*参数， **diskcomp**使用的当前驱动器*驱动器 2*。 如果省略这两个驱动器参数， **diskcomp**两个使用当前驱动器。 如果当前驱动器等同于*Drive1*， **diskcomp**会提示你根据需要交换磁盘。
-   使用一个驱动器

    如果指定的同一个软盘驱动器*Drive1*并*驱动器 2*， **diskcomp**使用一个驱动器比较，并提示您可以根据需要插入磁盘。 您可能必须交换磁盘超过一次，具体取决于磁盘和可用内存量的容量。
-   比较不同类型的磁盘

    **Diskcomp**无法比较具有双面的磁盘，也不具有双密度磁盘的高密度磁盘的单面磁盘。 如果在磁盘*Drive1*不是相同的类型中的磁盘*驱动器 2*， **diskcomp**会显示以下消息：  
    ```
    Drive types or diskette types not compatible
    ```  
-   使用**diskcomp**与网络和重定向的驱动器

    **Diskcomp**不起作用或创建的驱动器上的网络驱动器**subst**命令。 如果尝试使用**diskcomp**与任意这些类型的驱动器**diskcomp**显示以下错误消息：  
    ```
    Invalid drive specification
    ```  
-   比较具有副本的原始磁盘

    当你使用**diskcomp**所使用的磁盘**副本**， **diskcomp**可能会显示一条类似于以下消息：  
    ```
    Compare error on 
    side 0, track 0
    ```  
    即使在磁盘上的文件相同，则可以会出现此类错误。 尽管**复制**重复项的信息，它不会不一定是将其放在目标磁盘上的同一位置中。
-   了解**diskcomp**退出代码

    下表说明了每个退出代码。  
    |退出代码|描述|
    |---------|-----------|
    |0|是相同的磁盘|
    |1|发现差异|
    |3|发生硬错误|
    |4|出现初始化错误|

    进程退出代码，以返回到**diskcomp**，可以在使用 ERRORLEVEL 环境变量**如果**命令行中的批处理程序。

## <a name="BKMK_examples"></a>示例

如果您的计算机具有只有一个软盘驱动器 （例如，为驱动器 A），并且你想要比较两个磁盘，请键入：
```
diskcomp a: a:
```
**Diskcomp**将提示您插入每个磁盘，根据需要。

下面的示例演示如何处理**diskcomp**退出批处理程序在使用 ERRORLEVEL 环境变量中的代码**如果**命令行：
```
rem Checkout.bat compares the disks in drive A and B 
echo off 
diskcomp a: b: 
if errorlevel 4 goto ini_error 
if errorlevel 3 goto hard_error 
if errorlevel 1 goto no_compare
if errorlevel 0 goto compare_ok 
:ini_error 
echo ERROR: Insufficient memory or command invalid 
goto exit 
:hard_error 
echo ERROR: An irrecoverable error occurred 
goto exit 
:break 
echo "You just pressed CTRL+C" to stop the comparison 
goto exit 
:no_compare 
echo Disks are not the same 
goto exit 
:compare_ok 
echo The comparison was successful; the disks are the same 
goto exit 
:exit
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
