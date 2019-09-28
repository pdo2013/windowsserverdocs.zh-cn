---
title: nslookup server
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 608267f8-f7b4-412a-8dcd-e08b5ffc2085
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ce609f62f2d87024e1d75b43869b3b867e946e2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373065"
---
# <a name="nslookup-server"></a>nslookup server

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

将默认服务器更改为指定的域名系统（DNS）域。
## <a name="syntax"></a>语法
```
server <DNSDomain>
```
## <a name="parameters"></a>Parameters

|    参数    |                          描述                           |
|-----------------|----------------------------------------------------------------|
|   <DNSDomain>   | 必需。 为默认服务器指定新的 DNS 域。 |
| {help &#124; ？} |     显示**nslookup**子命令的简短摘要。      |

## <a name="remarks"></a>备注
- **服务器**命令使用当前的默认服务器来查找有关指定 DNS 域的信息。 这与使用初始服务器的**lserver**命令不同。
  ## <a name="additional-references"></a>其他参考
  [命令行语法 Key](command-line-syntax-key.md)
  [nslookup lserver](nslookup-lserver.md)
