---
title: clip
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 85322d85-3376-4806-845b-93ac77fe27bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 82186869782c47f41930d46b4c33a710e6addedf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379343"
---
# <a name="clip"></a>clip



将命令行中的命令输出重定向到 Windows 剪贴板。 然后，你可以将此文本输出粘贴到其他程序中。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
<Command> | clip
clip < <FileName>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<Command >|指定要将其输出发送到 Windows 剪贴板的命令。|
|\<文件名 >|指定要发送到 Windows 剪贴板的内容的文件。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

您可以使用**clip**命令将数据直接复制到任何可从剪贴板接收文本的应用程序。

## <a name="BKMK_examples"></a>示例

若要将当前目录列表复制到 Windows 剪贴板，请键入：
```
dir | clip
```
若要将名为的程序的输出复制到 Windows 剪贴板，请键入：
```
awk -f generic.awk input.txt | clip
```
若要将名为 readme.txt 的文件的内容复制到 Windows 剪贴板，请键入：
```
clip < readme.txt
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)