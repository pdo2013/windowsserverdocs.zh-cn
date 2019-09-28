---
title: bitsadmin getbytestransferred
description: 适用于**bitsadmin getbytestransferred**的 Windows 命令主题-检索为指定作业传输的字节数。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47bbf184-e06f-4be0-b2ba-d32b10d82002
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f690fa55a4ac5ae31223794c5e7eabc0c982c2ce
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381730"
---
# <a name="bitsadmin-getbytestransferred"></a>bitsadmin getbytestransferred



检索为指定作业传输的字节数。

## <a name="syntax"></a>语法

```
bitsadmin /GetBytesTransferred <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例

以下示例检索为名为*myDownloadJob*的作业传输的字节数。
```
C:\>bitsadmin /GetBytesTransferred myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)