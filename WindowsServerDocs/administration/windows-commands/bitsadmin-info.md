---
title: bitsadmin info
description: Windows 命令主题**显示有关指定作业的摘要信息。** -bitsadmin 信息
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c306677-0d64-41c0-8276-5bba7750cecb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ee96c69e311600a53f04b1b883983718adf0f69
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851518"
---
# <a name="bitsadmin-info"></a>bitsadmin info



显示有关指定的作业的摘要信息。

## <a name="syntax"></a>语法

```
bitsadmin /Info <Job> [/verbose]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="remarks"></a>备注

使用 /verbose 参数来提供有关作业的详细的信息。

## <a name="BKMK_examples"></a>示例

下面的示例检索名为的作业有关的信息*myDownloadJob*。
```
C:\>bitsadmin /Info myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)