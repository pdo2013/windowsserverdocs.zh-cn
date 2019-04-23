---
title: bitsadmin getreplyprogress
description: Windows 命令主题**bitsadmin getreplyprogress** -检索的大小和服务器回复的进度。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7f7cb0b4-ad95-44fd-a35d-0ddf5fc0b0d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aafecfb5873392ef86e6f7cceb139091b15e3b99
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852928"
---
# <a name="bitsadmin-getreplyprogress"></a>bitsadmin getreplyprogress

检索的大小和服务器回复的进度。

**1.2 及更早版本的 BITS**: 不支持。

## <a name="syntax"></a>语法

```
bitsadmin /GetReplyProgress <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="remarks"></a>备注

仅对上载-答复作业有效。

## <a name="BKMK_examples"></a>示例

下面的示例检索名为的作业的答复进度*myDownloadJob*。
```
C:\>bitsadmin /GetReplyProgress myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)