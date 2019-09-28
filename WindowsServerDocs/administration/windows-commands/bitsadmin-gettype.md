---
title: bitsadmin gettype
description: '**Bitsadmin gettype**的 Windows 命令主题-检索指定作业的作业类型。'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: ca46cb813809621f4fa79b3265198206729a392c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381336"
---
# <a name="bitsadmin-gettype"></a>bitsadmin gettype



检索指定作业的作业类型。

## <a name="syntax"></a>语法

```
bitsadmin /GetType <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="remarks"></a>备注

类型可以是 "下载"、"上载"、"上载-答复" 或 "未知"。

## <a name="BKMK_examples"></a>示例

下面的示例将检索名为*myDownloadJob*的作业的作业类型。
```
C:\>bitsadmin /GetType myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)