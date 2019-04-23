---
title: bitsadmin getproxylist-检索指定的作业的代理列表。
description: Windows 命令主题**bitsadmin getproxylist** -检索指定的作业的代理列表。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eebfa727-d8f1-4ae3-9382-6d8ffe8c3df3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e8c3ffb1e425552cda5b14a00287817ace77a90f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840508"
---
# <a name="bitsadmin-getproxylist"></a>bitsadmin getproxylist

检索指定的作业的代理列表。

## <a name="syntax"></a>语法

```
bitsadmin /GetProxyList <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="remarks"></a>备注

代理列表是要使用的代理服务器的列表。 列表是以逗号分隔。

## <a name="BKMK_examples"></a>示例

下面的示例检索名为的作业的代理列表*myDownloadJob*。
```
C:\>bitsadmin /GetProxyList myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)