---
title: clip
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: b10876e115c1f0dcac3448948003852449012087
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862388"
---
# <a name="clip"></a>clip



将重定向到 Windows 剪贴板从命令行命令输出。 然后，您可以将此文本输出粘贴到其他程序。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
<Command> | clip
clip < <FileName>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<命令 >|指定要发送到 Windows 剪贴板其输出的命令。|
|\<FileName>|指定你想要发送到 Windows 剪贴板的内容的文件。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

可以使用**剪辑**命令以将数据复制到任何应用程序都可以接收的文本从剪贴板直接。

## <a name="BKMK_examples"></a>示例

要复制的当前目录列表到 Windows 剪贴板，请键入：
```
dir | clip
```
若要复制一个名为到 Windows 剪贴板 Generic.awk 程序的输出，请键入：
```
awk -f generic.awk input.txt | clip
```
若要复制一个名为到 Windows 剪贴板的 Readme.txt 文件的内容，请键入：
```
clip < readme.txt
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)