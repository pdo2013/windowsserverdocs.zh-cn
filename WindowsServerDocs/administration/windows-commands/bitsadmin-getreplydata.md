---
title: bitsadmin getreplydata
description: 适用于**bitsadmin getreplydata**的 Windows 命令主题-以十六进制格式检索服务器的答复数据。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 819f97d5-b255-4b2d-9f63-0daa73915434
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7ebd3ee77e5d442467f49bb209c560f089f2271b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381276"
---
# <a name="bitsadmin-getreplydata"></a>bitsadmin getreplydata

检索以十六进制格式表示的服务器的答复数据。

**BITS 1.2 及更早版本**： 不受支持。

## <a name="syntax"></a>语法

```
bitsadmin /GetReplyData <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="remarks"></a>备注

仅对上传-答复作业有效。

## <a name="BKMK_examples"></a>示例

下面的示例将检索名为*myDownloadJob*的作业的答复数据。
```
C:\>bitsadmin /GetReplyData myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)