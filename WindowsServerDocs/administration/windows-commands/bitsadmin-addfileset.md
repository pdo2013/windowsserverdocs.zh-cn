---
title: bitsadmin addfileset
description: Windows 命令主题**bitsadmin addfileset** -将一个或多个文件添加到指定的作业。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f8f6ff32dfa6042272c68647477d77183ce9cb76
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889438"
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
|TextFile|文本文件，其中每一行都包含远程和本地文件名称。</br>注意：名称是以空格分隔。 # 字符开头的行被视为注释。|

## <a name="BKMK_examples"></a>示例

```
C:\>bitsadmin /addfileset files.txt
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)