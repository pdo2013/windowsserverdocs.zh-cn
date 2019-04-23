---
title: bitsadmin gettype
description: Windows 命令主题**bitsadmin gettype** -检索指定的作业的作业类型。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bec16f04-3e95-4587-889e-3de6ad03c9c8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ff0118f14acbf4e9f37c02e660bd9c7f6e8d0f70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879428"
---
# <a name="bitsadmin-gettype"></a>bitsadmin gettype



检索指定的作业的作业类型。

## <a name="syntax"></a>语法

```
bitsadmin /GetType <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="remarks"></a>备注

类型可以是下载上, 传，请上载-答复或未知。

## <a name="BKMK_examples"></a>示例

下面的示例检索名为的作业的作业类型*myDownloadJob*。
```
C:\>bitsadmin /GetType myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)