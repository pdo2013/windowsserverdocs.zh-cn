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
ms.openlocfilehash: 08cf41ec9b6ac30699013112216a538dcf625fd5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372844"
---
# <a name="nslookup-set-root"></a>nslookup set root

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

更改用于查询的根服务器的名称。
## <a name="syntax"></a>语法
```
set root=<RootServer>
```
## <a name="parameters"></a>Parameters

|    参数    |                                   描述                                    |
|-----------------|----------------------------------------------------------------------------------|
|  <RootServer>   | 指定根服务器的新名称。 默认值为 ns.nic.ddn.mil。 |
| {help &#124; ？} |              显示**nslookup**子命令的简短摘要。               |

## <a name="remarks"></a>备注
- **Set root**子命令影响**root**子命令。
  ## <a name="additional-references"></a>其他参考
  [命令行语法关键字](command-line-syntax-key.md)
  [nslookup 根](nslookup-root.md)
