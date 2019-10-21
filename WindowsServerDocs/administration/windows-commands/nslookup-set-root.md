---
title: nslookup set root
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8ad5393c-d4fd-4594-8187-576b1dcde60a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5a1737275bf6321525bbba56cd4d6a77ef973423
ms.sourcegitcommit: 9a6a692a7b2a93f52bb9e2de549753e81d758d28
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/18/2019
ms.locfileid: "72591025"
---
# <a name="nslookup-set-root"></a>nslookup set root

>适用于：Windows Server（半年频道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

更改用于查询的根服务器的名称。
## <a name="syntax"></a>语法
```
set root=<RootServer>
```
## <a name="parameters"></a>参数

|    参数    |                                   描述                                    |
|-----------------|----------------------------------------------------------------------------------|
|  <RootServer>   | 指定根服务器的新名称。 默认值为 ns.nic.ddn.mil。 |
| {help &#124; ？} |              显示**nslookup**子命令的简短摘要。               |

## <a name="remarks"></a>备注
- **Set root**子命令影响**root**子命令。
  ## <a name="additional-references"></a>其他参考
  [命令行语法键](command-line-syntax-key.md)
  [nslookup 根](nslookup-root.md)
