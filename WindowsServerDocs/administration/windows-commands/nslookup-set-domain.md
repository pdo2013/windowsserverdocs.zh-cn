---
title: nslookup set domain
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9d4d28e8-6e88-42cc-801f-94e9d8e051f4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9019893d92201079fb60b820a14dda3763bafd6b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886638"
---
# <a name="nslookup-set-domain"></a>nslookup set domain

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

更改默认域名系统 (DNS) 域名为指定的名称。
## <a name="syntax"></a>语法
```
set domain=<DomainName>
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|<DomainName>|指定的默认 DNS 域名的新名称。 默认域名是主机名。|
|{help &#124; ?}|显示的短摘要**nslookup**子命令。|
## <a name="remarks"></a>备注
-   默认 DNS 域的名称追加到查找请求的状态根据**defname**并**搜索**选项。 DNS 域搜索列表包含的默认 DNS 域的父项，如果它的名称中包含至少两个组件。 例如，如果 mfg.widgets.com 默认 DNS 域，则 mfg.widgets.com 和 widgets.com 命名的搜索列表。 使用**设置 srchlist**命令指定不同的列表并**设置所有**命令以显示的列表。
## <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[nslookup 设置 srchlist](nslookup-set-srchlist.md)
[nslookup 将所有设置](nslookup-set-all.md)
