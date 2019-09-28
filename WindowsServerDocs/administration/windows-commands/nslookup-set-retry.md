---
title: nslookup set retry
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 306bcc4f5e7ac98767c3c2e274100cf917874a8e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372856"
---
# <a name="nslookup-set-retry"></a>nslookup set retry

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

设置重试次数。
## <a name="syntax"></a>语法
```
set retry=<Number>
```
## <a name="parameters"></a>Parameters

|    参数    |                                      描述                                       |
|-----------------|----------------------------------------------------------------------------------------|
|    <Number>     | 指定重试次数的新值。 默认重试次数为4。 |
| {help &#124; ？} |                 显示**nslookup**子命令的简短摘要。                  |

## <a name="remarks"></a>备注
- 如果在特定时间段内未收到请求的答复，则超时期限将加倍，并重新发送请求。 重试值控制在放弃请求之前重新发送请求的次数。 您可以通过**设置超时**子命令更改超时期限。
  ## <a name="additional-references"></a>其他参考
  [命令行语法键](command-line-syntax-key.md)
  [nslookup 设置超时](nslookup-set-timeout.md)
