---
title: nslookup set recurse
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d1b7a93f-dfb0-4ccd-b230-e0953057fada
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68a5dc26387ddeb6541cc1c85005cd9dab4b433a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372895"
---
# <a name="nslookup-set-recurse"></a>nslookup set recurse



告诉域名系统（DNS）名称服务器查询其他服务器（如果没有此信息）。

## <a name="syntax"></a>语法

```
set [no]recurse
```

## <a name="parameters"></a>Parameters

|   参数   |                                                                  描述                                                                  |
|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| **norecurse** |                停止域名系统（DNS）名称服务器查询其他服务器（如果没有）。                |
|  **recurse**  | 告诉域名系统（DNS）名称服务器查询其他服务器（如果没有此信息）。 默认语法是**递归**的。 |
|     {帮助     |                                                                      ?}                                                                       |

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)