---
title: nslookup set querytype
description: '适用于 * * * * 的 Windows 命令主题 '
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
ms.openlocfilehash: 496eededd8b0b5eb79cdc1b4a7e35bc017157768
ms.sourcegitcommit: f3b61dcd8aa0aa744db4ea938aac633c19217b0a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2019
ms.locfileid: "70746301"
---
# <a name="nslookup-set-querytype"></a>nslookup set querytype

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

更改查询的资源记录类型。
## <a name="syntax"></a>语法
```
set querytype=<ResourceRecordtype>
```
## <a name="parameters"></a>Parameters
<ResourceRecordtype>指定 DNS 资源记录类型。 默认资源记录类型是。下表列出了此命令的有效值。

| ReplTest1 |                                                   描述                                                   |
|-------|-----------------------------------------------------------------------------------------------------------------|
|   A   |                                      指定计算机&#39;的 IP 地址                                      |
|  随时  |                                     指定计算机&#39;的 IP 地址。                                      |
| CNAME |                                    指定别名的规范名称。                                     |
|  GID  |                                  指定组名称的组标识符。                                  |
| HINFO |                          指定计算机&#39;的 CPU 和操作系统类型。                           |
|  MB   |                                        指定邮箱域名。                                         |
|  MG   |                                         指定邮件组成员。                                          |
| MINFO |                                   指定邮箱或邮件列表信息。                                   |
|  MR   |                                     指定邮件重命名域名。                                      |
|  MX   |                                          指定邮件交换器。                                          |
|  N   |                                 指定命名区域的 DNS 名称服务器。                                 |
|  PTR  | 如果查询是 IP 地址，则指定计算机名;否则，指定指向其他信息的指针。 |
|  SOA  |                                指定 DNS 区域的授权。                                 |
|  TXT  |                                         指定文本信息。                                         |
|  标识号  |                                         指定用户标识符。                                          |
| UINFO |                                         指定用户信息。                                         |
|  WKS  |                                         描述一个众所周知的服务。                                         |
| {帮助 |                                                       ?}                                                        |

显示<strong>nslookup</strong>子命令的简短摘要
## <a name="remarks"></a>备注
- <strong>Set type</strong>命令执行与<strong>set querytype</strong>命令相同的功能。
- 有关资源记录类型的详细信息，请参阅请求注释（Rfc）1035。
  ## <a name="additional-references"></a>其他参考
  <a href="command-line-syntax-key.md" data-raw-source="[Command-Line Syntax Key](command-line-syntax-key.md)">命令行语法密钥</a>
  <a href="nslookup-set-type.md" data-raw-source="[nslookup set type](nslookup-set-type.md)">nslookup 集类型</a>
