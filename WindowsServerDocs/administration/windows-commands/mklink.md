---
title: mklink
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0ce4df22-2dbc-48fc-9c16-b721ae85f857
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eabbf159a64ab5df7f45ece390d0c2fdb9956b80
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826328"
---
# <a name="mklink"></a>mklink
创建符号链接。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
mklink [[/d] | [/h] | [/j]] <Link> <Target>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/d|创建目录符号链接。 默认情况下**mklink**创建文件的符号链接。|
|/h|创建而不是符号链接的硬链接。|
|/j|创建一个目录接合点。|
|\<链接 >|指定要创建符号链接的名称。|
|\<Target>|指定新的符号链接引用的路径 （相对或绝对）。|
|/?|在命令提示符下显示帮助。|

## <a name="BKMK_examples"></a>示例

若要创建命名 MyDocs 从根目录到 \Users\User1\Documents 目录的符号链接，请键入：
```
mklink /d \MyDocs \Users\User1\Documents
```
## <a name="additional-references"></a>其他参考
-   [New-Item](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/new-item?view=powershell-6)
