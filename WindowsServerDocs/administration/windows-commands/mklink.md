---
title: mklink
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 9d930cbf7acbfceab16f2fa619aaaac6e789c131
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373638"
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
|/d|创建目录符号链接。 默认情况下， **mklink**创建文件符号链接。|
|/h|创建硬链接，而不是符号链接。|
|/j|创建目录连接。|
|\<Link >|指定正在创建的符号链接的名称。|
|\<Target >|指定新符号链接引用的路径（相对或绝对路径）。|
|/?|在命令提示符下显示帮助。|

## <a name="BKMK_examples"></a>示例

以下示例演示了如何在目录中创建一个名为 MyFolder 和 Myfile.txt 的符号链接，并将其从根目录中删除到 \Users\User1\Documents 目录和一个示例文件中：
```
mklink /d \MyFolder \Users\User1\Documents
mklink /h \MyFile.file \User1\Documents\example.file
rd \MyFolder
del \MyFile.file
```
## <a name="additional-references"></a>其他参考
-   [新建-项](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/new-item?view=powershell-6)
-   [del](https://docs.microsoft.com/windows-server/administration/windows-commands/del)
-   [rmdir](https://docs.microsoft.com/windows-server/administration/windows-commands/rd)
