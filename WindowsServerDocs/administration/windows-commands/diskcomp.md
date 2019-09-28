---
title: diskcomp
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: ca5ea0f4587b21b2a274c772aab239668b7868b4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377867"
---
# <a name="diskcomp"></a>diskcomp



比较两个软盘的内容。 如果在没有参数的情况下使用，则**diskcomp**将使用当前驱动器来比较两个磁盘。有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
diskcomp [<Drive1>: [<Drive2>:]]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<Drive1 >|指定包含其中一张软盘的驱动器。|
|\<Drive2 >|指定包含其他软盘的驱动器。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

- 使用磁盘

  **Diskcomp**命令仅适用于软盘。 不能将**diskcomp**用于硬盘。 如果为*Drive1*或*Drive2*指定硬盘驱动器，则**diskcomp**将显示以下错误消息：  
  ```
  Invalid drive specification
  Specified drive does not exist
  or is nonremovable
  ```  
- 比较磁盘

  如果两个磁盘上的所有磁道都是相同的，则**diskcomp**将显示以下消息：  
  ```
  Compare OK
  ```  
  如果曲目不相同， **diskcomp**将显示类似于以下内容的消息：  
  ```
  Compare error on
  side 1, track 2
  ```  
  当**diskcomp**完成比较时，将显示以下消息：  
  ```
  Compare another diskette (Y/N)?
  ```  
  如果按 "Y"， **diskcomp**会提示您插入磁盘以进行下一次比较。 如果按 N， **diskcomp**将停止比较。

  当**diskcomp**进行比较时，它将忽略磁盘的卷号。
- 省略驱动器参数

  如果省略*Drive2*参数，则**Diskcomp**将为*Drive2*使用当前驱动器。 如果省略这两个驱动器参数，则**diskcomp**将使用当前驱动器。 如果当前驱动器与*Drive1*相同，则**diskcomp**会提示你在必要时交换磁盘。
- 使用一个驱动器

  如果为*Drive1*和*Drive2*指定相同的软盘驱动器，则**diskcomp**将使用一个驱动器对它们进行比较，并根据需要提示插入磁盘。 可能需要多次交换磁盘，具体取决于磁盘容量和可用内存量。
- 比较不同类型的磁盘

  **Diskcomp**无法将单面磁盘与双面磁盘和具有双密度磁盘的高密度磁盘进行比较。 如果*Drive1*中的磁盘与*Drive2*中的磁盘不属于同一类型，则**diskcomp**将显示以下消息：  
  ```
  Drive types or diskette types not compatible
  ```  
- 将**diskcomp**与网络和重定向驱动器配合使用

  **Diskcomp**不适用于网络驱动器或由**subst**命令创建的驱动器。 如果尝试将**diskcomp**与任何这些类型的驱动器一起使用，则**diskcomp**将显示以下错误消息：  
  ```
  Invalid drive specification
  ```  
- 将原始磁盘与副本进行比较

  将**diskcomp**用于使用**copy**创建的磁盘时， **diskcomp**可能会显示类似于以下内容的消息：  
  ```
  Compare error on 
  side 0, track 0
  ```  
  即使磁盘上的文件相同，也可能出现这种类型的错误。 尽管**复制**重复的信息，但不一定要将其放置在目标磁盘上的相同位置。
- 了解**diskcomp**退出代码

  下表说明了每个退出代码。  

  |退出代码|描述|
  |---------|-----------|
  |0|磁盘相同|
  |1|发现差异|
  |3|出现硬错误|
  |4|出现初始化错误|

  若要处理**diskcomp**返回的退出代码，可以在批处理程序的**if**命令行中使用 ERRORLEVEL 环境变量。

## <a name="BKMK_examples"></a>示例

如果计算机只有一个软盘驱动器（例如驱动器 A），并且想要比较两个磁盘，请键入：
```
diskcomp a: a:
```
**Diskcomp**会根据需要提示插入每个磁盘。

下面的示例说明如何在**if**命令行中使用 ERRORLEVEL 环境变量的批处理程序中处理**diskcomp**退出代码：
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

[命令行语法项](command-line-syntax-key.md)
