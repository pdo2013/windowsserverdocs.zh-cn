---
title: bitsadmin addfileset
description: 适用于**bitsadmin addfileset**的 Windows 命令主题-将一个或多个文件添加到指定的作业。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 75466994-262f-4724-b14d-f813c5397675
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 464f2da151d5a7bfffde286e52d9158560d48dcc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381988"
---
# <a name="bitsadmin-addfileset"></a>bitsadmin addfileset

将一个或多个文件添加到指定的作业。

## <a name="syntax"></a>语法

```
bitsadmin /addfileset <Job> <TextFile>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|TextFile|一个文本文件，其中每行包含一个远程和一个本地文件名。</br>注意:名称以空格分隔。 以 # 字符开头的行被视为注释。|

## <a name="BKMK_examples"></a>示例

```
C:\>bitsadmin /addfileset files.txt
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)