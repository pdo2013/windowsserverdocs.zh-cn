---
title: edit
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4e0ff2e8-3518-47c1-8c69-5e93f895fa0e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 096632005b3e42dd941ccc7c72c08ead1d291b53
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873148"
---
# <a name="edit"></a>edit



启动 MS-DOS 编辑器中，将创建并更改 ASCII 文本文件。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
edit [/b] [/h] [/r] [/s] [/<NNN>] [[<Drive>:][<Path>]<FileName> [<FileName2> [...]]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[\<Drive>:][<Path>]<FileName> [<FileName2> [...]]|指定的位置和名称的一个或多个 ASCII 文本文件。 如果该文件不存在，则编辑器 MS-DOS 创建它。 如果该文件存在，MS-DOS 编辑器会将其打开，并在屏幕上显示其内容。 *文件名*可以包含通配符字符 (**&#42;** 并 **？**)。 用空格分隔多个文件的名称。|
|/b|强制单色模式，因此该 MS-DOS 编辑器在黑色和白色中显示。|
|/h|显示当前监视器的最大可能行数。|
|/r|加载在只读模式下的文件。|
|/s|强制使用短文件名。|
|\<NNN>|加载二进制文件，自动换行到*NNN*宽字符。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   更多帮助，打开 MS-DOS 编辑器，然后按 F1 键。
-   某些监视器默认情况下不支持键盘快捷方式的显示。 如果您的监视器不会显示键盘快捷方式，使用**b </b**。

## <a name="BKMK_examples"></a>示例

若要打开 MS-DOS 编辑器，请键入：
```
edit
```
若要创建和编辑名为 newtextfile.txt 当前目录中的文件，请键入：
```
edit newtextfile.txt
```