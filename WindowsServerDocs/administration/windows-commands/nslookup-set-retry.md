---
title: nslookup set retry
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 615fdfa2-fa29-47a8-8c9e-a6c5b45b3b71
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 876d8332e778aa0b3049354a21fbe01adb883729
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436648"
---
# <a name="nslookup-set-retry"></a>nslookup set retry

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

设置重试次数。
## <a name="syntax"></a>语法
```
set retry=<Number>
```
## <a name="parameters"></a>Parameters

|    参数    |                                      描述                                       |
|-----------------|----------------------------------------------------------------------------------------|
|    <Number>     | 指定重试次数的新值。 默认重试次数为 4。 |
| {help &#124; ?} |                 显示的短摘要**nslookup**子命令。                  |

## <a name="remarks"></a>备注
- 某段时间内没有收到对请求的回复，增加了一倍的超时期限，然后重新发送请求。 重试值控制在放弃之前重新发送请求的次数。 你可以使用的超时期限**超时设置**子命令。
  ## <a name="additional-references"></a>其他参考
  [命令行语法解答](command-line-syntax-key.md)
  [nslookup 设置超时](nslookup-set-timeout.md)
