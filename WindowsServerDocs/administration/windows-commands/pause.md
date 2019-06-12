---
title: pause
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cab3afc3-d046-432f-a0bf-6282f0099032
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e5805fcc14d6874d95ba90537d72b560229ba99b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436308"
---
# <a name="pause"></a>pause



挂起的批处理程序处理，并显示以下提示：
```
Press any key to continue . . .
```
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
pause
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

- 在运行时**暂停**命令时，将显示以下消息：  
  ```
  Press any key to continue . . .
  ```  
- 如果按 CTRL + C 来停止批处理程序时，将显示以下消息：  
  ```
  Terminate batch job (Y/N)?
  ```  
  如果对此消息的响应按 Y （为否)，批处理程序结束，控制将返回到操作系统。
- 可以插入**暂停**命令之前可能不想要处理的批处理文件的一部分。 当**暂停**挂起处理的批处理程序，您可以按 CTRL + C，然后按 Y，若要停止批处理程序。

## <a name="BKMK_examples"></a>示例

若要创建的批处理程序会提示用户更改其中一个驱动器中的磁盘，请键入：
```
@echo off 
:Begin 
copy a:*.* 
echo Put a new disk into drive A 
pause 
goto begin
```
在此示例中，在驱动器 a 中的磁盘上的所有文件都复制到当前目录。 消息，提示您将新磁盘驱动器 A 中后,**暂停**命令挂起处理，以便您可以更改磁盘，然后按任意键继续处理。 此批处理程序在运行进入无限循环 —**转到开始**命令将命令解释器发送到批处理文件的开始标签。 若要停止此批处理程序，按 CTRL + C，然后按 Y。

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)