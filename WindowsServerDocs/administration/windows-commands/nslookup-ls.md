---
title: nslookup ls
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f15f06fe-67e7-41a9-93b5-192ab14ab380
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6a11867ff2ec69b1ef938149ac485ff8827b58de
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436919"
---
# <a name="nslookup-ls"></a>nslookup ls

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

列出了域名系统 (DNS) 域的信息。
## <a name="syntax"></a>语法
```
ls [<Option>] <DNSDomain> [{[>] <FileName>|[>>] <FileName>}]
```
## <a name="parameters"></a>Parameters

|    参数    |                                                                                                                                                                                                                                                                                                               描述                                                                                                                                                                                                                                                                                                                |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <Option>     | 下表列出了有效的选项。<br /><br />--t： 列出指定类型的所有记录。 有关的说明<querytype>，请参阅**setquerytype**中其他引用。<br />--a： 列出了别名的 DNS 域中的计算机。 此参数是的同义词 **-t CNAME**<br />--d： 列出为 DNS 域的所有记录。 此参数是的同义词 **-t ANY**<br />--h： 列出的 DNS 域的 CPU 和操作系统信息。 此参数是的同义词 **-t HINFO**<br />--s： 列出的已知服务的 DNS 域中的计算机。 此参数是的同义词 **-t WKS**。 |
|   <DNSDomain>   |                                                                                                                                                                                                                                                                                         指定要为其信息的 DNS 域。                                                                                                                                                                                                                                                                                         |
|   <FileName>    |                                                                                                                                                                                                                                 指定要在其中保存输出的文件名称。 可以使用大于 (>) 和双精度大于 (>>) 个字符将输出重定向以通常的方式。                                                                                                                                                                                                                                  |
| {help &#124; ?} |                                                                                                                                                                                                                                                                                          显示的短摘要**nslookup**子命令。                                                                                                                                                                                                                                                                                           |

## <a name="remarks"></a>备注
- 默认输出包含计算机名称及其 IP 地址。 当输出定向到某个文件时，从服务器收到每 50 条记录打印哈希符号
  ## <a name="additional-references"></a>其他参考
  [命令行语法解答](command-line-syntax-key.md)
  [nslookup 设置 querytype](nslookup-set-querytype.md)
