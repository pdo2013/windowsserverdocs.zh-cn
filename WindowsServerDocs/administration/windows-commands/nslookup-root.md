---
title: nslookup root
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9c29edc3-ec49-43f2-bc49-86bf0612d816
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3eb3375df3a109685fc8dc5d23f0c5008339d09e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373387"
---
# <a name="nslookup-root"></a>nslookup root

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

将服务器的默认服务器更改为域名系统（DNS）域名空间的根。
## <a name="syntax"></a>语法
```
root 
```
## <a name="parameters"></a>Parameters

|    参数    |                      描述                      |
|-----------------|-------------------------------------------------------|
| {help &#124; ？} | 显示**nslookup**子命令的简短摘要。 |

## <a name="remarks"></a>备注
- 目前，使用了 ns.nic.ddn.mil 名称服务器。 此命令是 lserver ns.nic.ddn.mil 的同义词。 可以通过 "**设置根**" 命令更改根服务器的名称。
  ## <a name="additional-references"></a>其他参考
  [命令行语法关键字](command-line-syntax-key.md)
  [nslookup 设置根](nslookup-set-root.md)
