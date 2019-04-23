---
title: bitsadmin getproxybypasslist
description: Windows 命令主题**bitsadmin getproxybypasslist** -检索指定的作业代理跳过列表。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 50959be3-7014-4bc9-9a7b-68f1ff94a94a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 020b8fc0019eb103a0e469258be8705b80dd45de
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854108"
---
# <a name="bitsadmin-getproxybypasslist"></a>bitsadmin getproxybypasslist

检索指定的作业代理跳过列表。

## <a name="syntax"></a>语法

```
bitsadmin /GetProxyBypassList <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="remarks"></a>备注

跳过列表包含的主机名或 IP 地址和 / 或，不会通过代理路由。 列表可以包含"\<本地 >"来指代同一 LAN 上的所有服务器。 该列表可以为以分号或空格分隔。

## <a name="BKMK_examples"></a>示例

下面的示例检索名为的作业代理跳过列表*myDownloadJob*。
```
C:\>bitsadmin /GetProxyBypassList myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)