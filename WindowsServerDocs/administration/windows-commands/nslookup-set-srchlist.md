---
title: nslookup set srchlist
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8486266d-22ac-4ce5-aad6-1cd0c08110a2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 39b28e7d43df2427caae46d323cd30f03b6b484c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436571"
---
# <a name="nslookup-set-srchlist"></a>nslookup set srchlist

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

更改默认域名系统 (DNS) 域的名称和搜索列表。

## <a name="syntax"></a>语法
```
Set srchlist=<DomainName>[/...]
```
## <a name="parameters"></a>Parameters

|    参数    |                                                                                        描述                                                                                        |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <DomainName>   | 指定默认 DNS 域和搜索列表的新的名称。 默认域名值基于主机名。 您可以指定最多六个由斜杠 （/） 分隔的名称。 |
| {help &#124; ?} |                                                                   显示的短摘要**nslookup**子命令。                                                                   |

## <a name="remarks"></a>备注
- **设置 srchlist**命令将重写的默认 DNS 域名称和搜索列表**集域**命令。 使用**设置所有**命令以显示的列表。
  ## <a name="BKMK_examples"></a>示例
  下面的示例将 DNS 域设置为 mfg.widgets.com 和三个名称的搜索列表：
  ```
  set srchlist=mfg.widgets.com/mrp2.widgets.com/widgets.com
  ```
  ## <a name="additional-references"></a>其他参考
  [命令行语法解答](command-line-syntax-key.md)
  [nslookup 设置域](nslookup-set-domain.md)
  [nslookup 将所有设置](nslookup-set-all.md)
