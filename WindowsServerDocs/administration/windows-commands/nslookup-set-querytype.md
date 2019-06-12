---
title: nslookup set querytype
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5af54ac5-fc1a-4af6-977b-f8e97c8eba90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f0015db716bd8c74bc4366063009bda41d338d19
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436737"
---
# <a name="nslookup-set-querytype"></a>nslookup set querytype

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

更改查询的资源记录类型。
## <a name="syntax"></a>语法
```
set querytype=<ResourceRecordtype>
```
## <a name="parameters"></a>Parameters
<ResourceRecordtype> 指定 DNS 资源记录类型。 默认资源记录类型为 a。下表列出了此命令有效的值。

| ReplTest1 |                                                   描述                                                   |
|-------|-----------------------------------------------------------------------------------------------------------------|
|   A   |                                      指定的计算机&#39;IP 地址                                      |
|  任何  |                                     指定的计算机&#39;IP 地址。                                      |
| CNAME |                                    指定别名的规范名称。                                     |
|  GID  |                                  指定组名称的组标识符。                                  |
| HINFO |                          指定的计算机&#39;的 CPU 和操作系统的类型。                           |
|  MB   |                                        指定邮箱的域名。                                         |
|  MG   |                                         指定邮件的组成员。                                          |
| MINFO |                                   指定邮箱或邮件的列表信息。                                   |
|  MR   |                                     指定邮件重命名域的名称。                                      |
|  MX   |                                          指定邮件交换器。                                          |
|  NS   |                                 指定命名区域的 DNS 名称服务器。                                 |
|  PTR  | 指定的计算机命名查询是 IP 地址; 如果否则，指定了指向其他信息。 |
|  SOA  |                                指定为 DNS 区域的机构开始。                                 |
|  TXT  |                                         指定的文本信息。                                         |
|  UID  |                                         指定的用户标识符。                                          |
| UINFO |                                         指定的用户信息。                                         |
|  WKS  |                                         介绍了已知的服务。                                         |
| {帮助 |                                                       ?}                                                        |

显示的短摘要<strong>nslookup</strong>子命令
## <a name="remarks"></a>备注
- <strong>类型设置</strong>命令执行相同的功能<strong>设置 querytype</strong>命令。
- 有关资源记录类型的详细信息，请参阅注释 (Rfc) 1035年的请求。
  ## <a name="additional-references"></a>其他参考
  <a href="command-line-syntax-key.md" data-raw-source="[Command-Line Syntax Key](command-line-syntax-key.md)">命令行语法解答</a>
  <a href="nslookup-set-type.md" data-raw-source="[nslookup set type](nslookup-set-type.md)">nslookup 设置类型</a>
