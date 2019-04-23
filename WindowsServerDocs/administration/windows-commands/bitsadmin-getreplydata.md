---
title: bitsadmin getreplydata
description: Windows 命令主题**bitsadmin getreplydata** -检索以十六进制格式的服务器的回复数据。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 78d70a44d6881568c8d92db145fdf22a260ee8af
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883818"
---
# <a name="bitsadmin-getreplydata"></a>bitsadmin getreplydata

检索以十六进制格式的服务器的回复数据。

**1.2 及更早版本的 BITS**: 不支持。

## <a name="syntax"></a>语法

```
bitsadmin /GetReplyData <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="remarks"></a>备注

仅对上载-答复作业有效。

## <a name="BKMK_examples"></a>示例

下面的示例检索名为的作业的回复数据*myDownloadJob*。
```
C:\>bitsadmin /GetReplyData myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)