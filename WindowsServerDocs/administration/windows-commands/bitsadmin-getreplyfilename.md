---
title: bitsadmin getreplyfilename
description: Windows 命令主题**bitsadmin getreplyfilename** -获取包含服务器回复的文件的路径。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 85447184-1732-4816-a365-2e3599551bf8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 46130700e9ac7e2d0076b368712e5dcb3f02ba2f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862148"
---
# <a name="bitsadmin-getreplyfilename"></a>bitsadmin getreplyfilename

获取包含服务器回复的文件的路径。

**1.2 及更早版本的 BITS**: 不支持。

## <a name="syntax"></a>语法

```
bitsadmin /GetReplyFileName <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="remarks"></a>备注

仅对上载-答复作业有效。

## <a name="BKMK_examples"></a>示例

下面的示例检索名为的作业的答复文件名*myDownloadJob*。
```
C:\>bitsadmin /GetReplyFileName myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)