---
title: recover
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cf9be2e3-90c8-4773-a201-dc503b91948e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 805f63e95bcb72416cdacea4ba792af8c9a96c06
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813098"
---
# <a name="recover"></a>recover



从损坏的磁盘恢复可读信息。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
recover [<Drive>:][<Path>]<FileName>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[\<Drive>:][<Path>]<FileName>|指定的位置和你想要恢复的文件的名称。 *文件名*是必需的。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   **恢复**命令读取文件，扇区的区域，并很好的扇区从恢复数据。 坏扇区中的数据都将丢失。
-   坏扇区报告**chkdsk**磁盘准备操作时被标记为"错误"。 它们不会造成危险，并**恢复**不会影响它们。
-   因为坏扇区中的所有数据丢失时恢复的文件时，应恢复一次只有一个文件。
-   不能使用通配符字符 (**&#42;** 并 **？**) 与**恢复**命令。 必须指定一个文件 （和如果它不是当前目录中的文件的位置）。

## <a name="BKMK_examples"></a>示例

若要恢复 Story.txt 目录中的文件 \Fiction 驱动器 D 上，键入：
```
recover d:\fiction\story.txt 
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)