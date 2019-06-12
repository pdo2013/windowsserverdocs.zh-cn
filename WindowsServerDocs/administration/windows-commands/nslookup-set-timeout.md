---
title: nslookup set timeout
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 07afdaf4-ffec-496f-a188-4e91cf1a28f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1f6c8863d0a9330fd3a8499b0e6dbc802bd95022
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436517"
---
# <a name="nslookup-set-timeout"></a>nslookup set timeout

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

更改初始的查找请求的答复到等待秒的数。
## <a name="syntax"></a>语法
```
set timeout=<Number>
```
## <a name="parameters"></a>Parameters

|    参数    |                                           描述                                            |
|-----------------|--------------------------------------------------------------------------------------------------|
|    <Number>     | 指定要等待回复的秒数。 等待的秒的默认数目为 5。 |
| {help &#124; ?} |                      显示的短摘要**nslookup**子命令。                       |

## <a name="remarks"></a>备注
- 在指定的时间段内未收到对请求的回复，超时值加倍，然后再次发送该请求。 可以使用**集重试**命令来控制重试次数。
  ## <a name="BKMK_examples"></a>示例
  下面的示例设置用于获取对 2 秒的响应的超时值：
  ```
  set timeout=2
  ```
  ## <a name="additional-references"></a>其他参考
  [命令行语法解答](command-line-syntax-key.md)
  [nslookup 设置重试](nslookup-set-retry.md)
