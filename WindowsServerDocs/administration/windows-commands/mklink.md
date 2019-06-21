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
ms.openlocfilehash: 1ea80b81b268b31f637c72a828fee8b6f0229a47
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280027"
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

以下示例演示了创建和删除的名为 MyFolder 和 MyFile.file 从根目录到 \Users\User1\Documents 目录的符号链接和 example.file 位于该目录中：
```
mklink /d \MyFolder \Users\User1\Documents
mklink /h \MyFile.file \User1\Documents\example.file
rd \MyFolder
del \MyFile.file
```
## <a name="additional-references"></a>其他参考
-   [New-Item](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/new-item?view=powershell-6)
-   [del](https://docs.microsoft.com/windows-server/administration/windows-commands/del)
-   [rmdir](https://docs.microsoft.com/windows-server/administration/windows-commands/rd)
