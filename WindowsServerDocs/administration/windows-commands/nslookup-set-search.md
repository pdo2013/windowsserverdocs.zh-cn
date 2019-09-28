---
title: nslookup set search
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 064ac660-8b04-4af9-8b2c-e4e0549771b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d9da08a296d61789dbafeccde5d46c8a220d874c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372776"
---
# <a name="nslookup-set-search"></a>nslookup set search



向请求追加 DNS 域搜索列表中的域名系统（DNS）域名，直到接收到答案。 这适用于以下情况：集和查找请求至少包含一个句点，但不以尾随句点结束。

## <a name="syntax"></a>语法

```
set [no]search
```

## <a name="parameters"></a>Parameters

|  参数   |                                                                          描述                                                                          |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **nosearch** |                            停止将 DNS 域搜索列表中的域名系统（DNS）域名追加到该请求。                            |
|  **寻找**  | 向请求追加 DNS 域搜索列表中的域名系统（DNS）域名，直到接收到答案。 默认语法为 "**搜索**"。 |
|    {帮助     |                                                                              ?}                                                                               |

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)