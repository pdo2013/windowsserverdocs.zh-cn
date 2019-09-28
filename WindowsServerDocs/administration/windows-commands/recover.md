---
title: recover
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 415efe2d1e60ca70d68059b5702108440da735f3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371768"
---
# <a name="recover"></a>recover



从错误或有缺陷的磁盘恢复可读的信息。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
recover [<Drive>:][<Path>]<FileName>
```

## <a name="parameters"></a>Parameters

|           参数           |                                          描述                                          |
|-------------------------------|-----------------------------------------------------------------------------------------------|
| [@no__t >：][<Path>] <FileName> | 指定要恢复的文件的位置和名称。 *FileName*是必需的。 |
|              /?               |                             在命令提示符下显示帮助。                              |

## <a name="remarks"></a>备注

-   **Recover**命令逐扇区读取文件，并从良好的扇区恢复数据。 坏扇区中的数据将丢失。
-   为操作准备好磁盘时， **chkdsk**报告的坏扇区被标记为 "错误"。 它们不会带来任何风险，**恢复**不会对其造成影响。
-   由于在您恢复文件时，坏扇区中的所有数据都将丢失，因此，一次只能恢复一个文件。
-   不能将通配符（ **&#42;** 和 **？** ）与**recover**命令一起使用。 如果文件不在当前目录中，则必须指定文件（以及文件的位置）。

## <a name="BKMK_examples"></a>示例

若要在驱动器 D 上的 \Fiction 目录中恢复文件故事 .txt，请键入：
```
recover d:\fiction\story.txt 
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)