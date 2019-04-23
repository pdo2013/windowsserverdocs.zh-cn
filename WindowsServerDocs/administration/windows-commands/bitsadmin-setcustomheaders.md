---
title: bitsadmin setcustomheaders
description: Windows 命令主题**bitsadmin setcustomheaders** -将自定义 HTTP 标头添加到 GET 请求。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed926410-80d0-46ed-9a90-f752c164bb9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6d90ac2d23b852ae0c2114e7cd5a9c9e6382ce8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853848"
---
# <a name="bitsadmin-setcustomheaders"></a>bitsadmin setcustomheaders



将自定义 HTTP 标头添加到 GET 请求。

## <a name="syntax"></a>语法

```
bitsadmin /SetCustomHeaders <Job> <Header1> <Header2> <. . .>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|Header1 Header2。 . .|作业的自定义标头|

## <a name="remarks"></a>备注

-   若要将自定义 HTTP 标头添加到 GET 请求发送到 HTTP 服务器，使用此开关。

## <a name="BKMK_examples"></a>示例

下面的示例添加名为的作业的自定义 HTTP 标头*myDownloadJob*。
```
C:\>bitsadmin / SetCustomHeaders myDownloadJob "Accept-encoding:deflate/gzip"
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)