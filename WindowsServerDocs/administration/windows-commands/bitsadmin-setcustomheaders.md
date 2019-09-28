---
title: bitsadmin setcustomheaders
description: 适用于**bitsadmin setcustomheaders**的 Windows 命令主题-将自定义 HTTP 标头添加到 GET 请求。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 45e3a5178df69b84618966ca0fcd9cc1e6d0e449
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380638"
---
# <a name="bitsadmin-setcustomheaders"></a>bitsadmin setcustomheaders



向 GET 请求添加自定义 HTTP 标头。

## <a name="syntax"></a>语法

```
bitsadmin /SetCustomHeaders <Job> <Header1> <Header2> <. . .>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|Header1 .Header2。 . .|作业的自定义标头|

## <a name="remarks"></a>备注

-   此开关用于向发送到 HTTP 服务器的 GET 请求添加自定义 HTTP 标头。

## <a name="BKMK_examples"></a>示例

以下示例添加名为*myDownloadJob*的作业的自定义 HTTP 标头。
```
C:\>bitsadmin / SetCustomHeaders myDownloadJob "Accept-encoding:deflate/gzip"
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)