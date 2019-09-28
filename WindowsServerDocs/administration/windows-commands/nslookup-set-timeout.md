---
title: nslookup set timeout
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 32fcfcaeccb6599e9aaca21f9c085bb00857479c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372763"
---
# <a name="nslookup-set-timeout"></a>nslookup set timeout

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

更改等待查找请求回复的初始秒数。
## <a name="syntax"></a>语法
```
set timeout=<Number>
```
## <a name="parameters"></a>Parameters

|    参数    |                                           描述                                            |
|-----------------|--------------------------------------------------------------------------------------------------|
|    <Number>     | 指定等待答复的秒数。 默认等待的秒数为5。 |
| {help &#124; ？} |                      显示**nslookup**子命令的简短摘要。                       |

## <a name="remarks"></a>备注
- 如果在指定的时间段内未收到对请求的答复，则超时将加倍，并再次发送请求。 你可以使用 "**设置重试**" 命令来控制重试的次数。
  ## <a name="BKMK_examples"></a>示例
  下面的示例将获取响应的超时时间设置为2秒：
  ```
  set timeout=2
  ```
  ## <a name="additional-references"></a>其他参考
  [命令行语法关键字](command-line-syntax-key.md)
  [nslookup 设置重试](nslookup-set-retry.md)
